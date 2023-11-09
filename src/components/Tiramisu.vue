<template>
  <div>
    <svg :width="images.backdrop.width" :height="images.backdrop.height" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <marker id="arrowhead-0" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0" /></marker>
        <marker id="arrowhead-0.1" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.1" /></marker>
        <marker id="arrowhead-0.2" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.2" /></marker>
        <marker id="arrowhead-0.3" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.3" /></marker>
        <marker id="arrowhead-0.4" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.4" /></marker>
        <marker id="arrowhead-0.5" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.5" /></marker>
        <marker id="arrowhead-0.6" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.6" /></marker>
        <marker id="arrowhead-0.7" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.7" /></marker>
        <marker id="arrowhead-0.8" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.8" /></marker>
        <marker id="arrowhead-0.9" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.9" /></marker>
        <marker id="arrowhead-1" markerWidth="3" markerHeight="2" refX="0" refY="1" orient="auto"><polygon points="0 0, 2 1, 0 2" opacity="0.9" /></marker>
      </defs>

      <!-- backdrop -->
      <image v-if="showBackdrop" :href="images.backdrop.pictogram" />

      <!-- activities -->
      <g v-if="showActivities" v-for="(action, name, index) in images.actions" :key="name">
        <image v-if="name in model.actions" :href="action.pictogram" :x="action.x" :y="action.y" :style="intensityActivity(model.actions[name].intensity)" />
      </g>

      <!-- edges -->
      <g v-for="(edge, index) in model.edges" :key="index">
        <path
            v-if="showEdges"
            :d="getCoordinates(
              images.actions[edge.from].x + (images.actions[edge.from].w / 2),
              images.actions[edge.from].y + (images.actions[edge.from].h / 2),
              images.actions[edge.to].x + (images.actions[edge.to].w / 2),
              images.actions[edge.to].y + (images.actions[edge.to].h / 2))"
            fill="none"
            stroke="black"
            :stroke-width="edge.intensity * 20"
            :stroke-opacity="edge.intensity"
            :marker-end="'url(#arrowhead-'+ Math.round(edge.intensity * 10)/10 +')'"
        />
      </g>

      <!-- activity labels -->
      <g v-for="(action, name, index) in images.actions" :key="name">
        <text
            v-if="showActivityLabels && name in model.actions"
            :x="action.x + (action.w / 2) - 50"
            :y="action.y + (action.h / 2)"
            style="stroke-width: 4; stroke:white; stroke-opacity:1; paint-order: stroke;">
          <tspan>{{ name }}</tspan>
          <tspan dy="1.2em" :x="action.x + (action.w / 2) - 50" style="font-size: smaller">Freq.: {{ Math.ceil(model.actions[name].intensity * 100)/100 }}</tspan>
        </text>
      </g>

      <!-- activity links -->
      <g v-for="(action, name, index) in images.actions" :key="name">
        <rect
            v-if="name in model.actions"
            :x="action.x" :y="action.y"
            :width="action.w" :height="action.h"
            class="activityRectangle"
            data-bs-toggle="modal"
            data-bs-target="#SeeDetailsModal"
            @click="displayContent(name)"
        />
      </g>
    </svg>
    <div class="modal fade" id="SeeDetailsModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered modal-lg modal-dialog-scrollable">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Details for <code>{{ this.activeAction }}</code></h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <div id="modal-content"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tiramisu',
  props: {
    showBackdrop: true,
    showActivities: true,
    showActivityLabels: true,
    showActivityShadows: true,
    showEdges: true,
    configuration: null,
    dfg: null
  },
  data: () => ({
    images: {
      backdrop: {
        pictogram: null,
        width: 0,
        height: 0
      },
    },
    model: {
      actions: {},
      edges: []
    },
    activeAction: null
  }),
  methods: {
    refresh() {
      if (this.configuration == null || this.dfg == null)
        return;
      this.parseConfiguration(this.configuration);
      this.parseDFG(this.dfg);
    },
    async parseConfiguration(url) {
      const response = await fetch(url);
      const json = await response.json();
      this.images = json;
    },
    async parseDFG(url) {
      let i;
      const response = await fetch(url);
      const text = await response.text();
      const lines = text.split('\n');

      const num_activities = parseInt(lines[0]);
      let most_frequent_activity = 1;
      let most_frequent_edge = 1;

      let activity_to_frequency = {};
      let activity_from_frequency = {};

      for (i = 1; i < num_activities + 1; i++) {
        activity_to_frequency[lines[i]] = 0;
        activity_from_frequency[lines[i]] = 0;
      }

      // extract the most frequent edge and all activities frequencies
      for (i = num_activities + 1; i < lines.length; i++) {
        if (lines[i].includes('>')){
          const edge = lines[i].split('>');
          const from = parseInt(edge[0]);
          const second_part = edge[1].split('x');
          const to = parseInt(second_part[0]);
          const count = parseInt(second_part[1]);
          const activity_from_name = lines[from + 1];
          const activity_to_name = lines[to + 1];

          most_frequent_edge = Math.max(most_frequent_edge, count);
          activity_to_frequency[activity_to_name] += count;
          activity_from_frequency[activity_from_name] += count;
          most_frequent_activity = Math.max(most_frequent_activity, activity_to_frequency[activity_to_name]);
          most_frequent_activity = Math.max(most_frequent_activity, activity_from_frequency[activity_from_name]);
        }
      }

      // set activities intensity
      for (const [activity_name, value] of Object.entries(this.images.actions)) {
        const activity_frequency = Math.max(activity_to_frequency[activity_name], activity_from_frequency[activity_name]);
        this.model.actions[activity_name] = { intensity: activity_frequency / most_frequent_activity };
      }

      // set edges intensity
      for (i = num_activities + 1; i < lines.length; i++) {
        if (lines[i].includes('>')){
          const edge = lines[i].split('>');
          const from = parseInt(edge[0]);
          const second_part = edge[1].split('x');
          const to = parseInt(second_part[0]);
          const count = parseInt(second_part[1]);
          const activity_from_name = lines[from + 1];
          const activity_to_name = lines[to + 1];

          this.model.edges.push({
            from: activity_from_name,
            to: activity_to_name,
            intensity: count / most_frequent_edge
          });
        }
      }
    },
    async displayContent(name) {
      this.activeAction = name;
      const url = this.images.actions[name].info;
      const response = await fetch(url);
      const text = await response.text();
      document.getElementById("modal-content").innerHTML = text;
    },
    intensityActivity(intensity) {
      if ((!Number.isNaN(intensity)) && this.showActivityShadows)
        return 'filter: drop-shadow(0px 0px '+ (intensity*20) +'px green)';
    },
    getCoordinates(x1, y1, x2, y2) {
      const size = 40;
      const angle = Math.atan2(y2 - y1, x2 - x1);
      const newX1 = x1 + size * Math.cos(angle);
      const newY1 = y1 + size * Math.sin(angle);
      const newX2 = x2 - size * Math.cos(angle);
      const newY2 = y2 - size * Math.sin(angle);
      // define control points for a quadratic curve
      const control = this.calculateControlPoint(newX1, newY1, newX2, newY2, 0.2);
      // spit out the path of the quadratic curve for the svg
      return 'M ' + newX1 + ' ' + newY1 + ' Q ' + control['x'] + ' ' + control['y'] + ' ' + newX2 + ' ' + newY2;
    },
    calculateControlPoint(x1, y1, x2, y2, bendFactor) {
      // Calculate the mid-point
      const midX = (x1 + x2) / 2;
      const midY = (y1 + y2) / 2;
      // Calculate the vector representing the direction of the line
      const dx = x2 - x1;
      const dy = y2 - y1;
      // Rotate the vector 90 degrees to create a perpendicular vector
      const perpendicularX = -dy;
      const perpendicularY = dx;
      // Scale the perpendicular vector by the bendFactor
      const scaledX = midX + (perpendicularX * bendFactor);
      const scaledY = midY + (perpendicularY * bendFactor);
      return { x: scaledX, y: scaledY };
    }
  },
  watch: {
    configuration: function() { this.refresh() },
    dfg: function() { this.refresh() }
  }
}
</script>

<style scoped>
.activityRectangle {
  fill: none;
  pointer-events: all;
  stroke-width: 0;
  stroke: red;
}
  .activityRectangle:hover {
    stroke-width: 5;
  }
</style>
