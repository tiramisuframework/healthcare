<template>
  <div>
    <svg :width="dimensions.width" :height="dimensions.height" xmlns="http://www.w3.org/2000/svg">
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
      <image v-if="showBackdrop" :href="images.backdropUrl" />

      <!-- activities -->
      <g v-if="showActivities" v-for="(action, name, index) in images.actions" :key="name">
        <image v-if="name in model.actions" :href="action.pictogram" :x="action.x" :y="action.y" :style="intensityActivity(model.actions[name].intensity)" />
      </g>

      <!-- edges -->
      <g v-for="(edge, index) in model.edges" :key="index">
        <line
          v-if="showEdges"
          :x1="getCoordinates(
              images.actions[edge.from].x + (images.actions[edge.from].w / 2),
              images.actions[edge.from].y + (images.actions[edge.from].h / 2),
              images.actions[edge.to].x + (images.actions[edge.to].w / 2),
              images.actions[edge.to].y + (images.actions[edge.to].h / 2))['x1']"
          :y1="getCoordinates(
              images.actions[edge.from].x + (images.actions[edge.from].w / 2),
              images.actions[edge.from].y + (images.actions[edge.from].h / 2),
              images.actions[edge.to].x + (images.actions[edge.to].w / 2),
              images.actions[edge.to].y + (images.actions[edge.to].h / 2))['y1']"
          :x2="getCoordinates(
              images.actions[edge.from].x + (images.actions[edge.from].w / 2),
              images.actions[edge.from].y + (images.actions[edge.from].h / 2),
              images.actions[edge.to].x + (images.actions[edge.to].w / 2),
              images.actions[edge.to].y + (images.actions[edge.to].h / 2))['x2']"
          :y2="getCoordinates(
              images.actions[edge.from].x + (images.actions[edge.from].w / 2),
              images.actions[edge.from].y + (images.actions[edge.from].h / 2),
              images.actions[edge.to].x + (images.actions[edge.to].w / 2),
              images.actions[edge.to].y + (images.actions[edge.to].h / 2))['y2']"
          :stroke-width="edge.intensity * 20"
          :stroke-opacity="edge.intensity"
          stroke="black"
          :marker-end="'url(#arrowhead-'+ Math.round(edge.intensity * 10)/10 +')'"
        />
      </g>

      <!-- activities -->
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
    </svg>
  </div>
</template>

<script>
export default {
  name: 'Tiramisu',
  props: {
    showBackdrop: true,
    showActivities: true,
    showActivityLabels: true,
    showEdges: true
  },
  data: () => ({
    dimensions: {
      width: 800,
      height: 508
    },
    images: {
      backdropUrl: 'https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/backdrop.png',
      actions: {
        "entrance": { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/entrance.png", x:732,y:228, w: 64,h: 63 },
        "living":   { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/living.png",   x:181,y:305, w:346,h:190 },
        "bedroom":  { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/bedroom.png",  x:535,y:338, w:253,h:158 },
        "bathroom": { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/bathroom.png", x:549,y:14,  w:117,h:192 },
        "kitchen":  { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/kitchen.png",  x:171,y:14,  w:360,h:221 }
      },
    },
    model: {
      actions: {
        // "entrance": { intensity: 1 },
        // "living": {intensity: 0.5}
      },
      edges: [
        // { from: "entrance", to: "living",  intensity: 0.5 },
        // { from: "living",   to: "kitchen", intensity: 1 }
      ]
    },
    dfg: "https://raw.githubusercontent.com/delas/tiramisu/master/examples/example1.dfg",
  }),
  methods: {
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
    intensityActivity(intensity) {
      if (!Number.isNaN(intensity))
        return 'filter: drop-shadow(0px 0px '+ (intensity*20) +'px green)';
    },
    getCoordinates(x1, y1, x2, y2) {
      const size = 20;
      const angle = Math.atan2(y2 - y1, x2 - x1);
      const newX1 = x1 + size * Math.cos(angle);
      const newY1 = y1 + size * Math.sin(angle);
      const newX2 = x2 - size * Math.cos(angle);
      const newY2 = y2 - size * Math.sin(angle);
      return {x1: newX1, y1: newY1, x2: newX2, y2: newY2};
    },
  },
  mounted() {
    this.parseDFG(this.dfg);
  }
}
</script>
