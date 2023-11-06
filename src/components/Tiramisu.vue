<template>
  <div>
    <svg :width="dimensions.width" :height="dimensions.height" xmlns="http://www.w3.org/2000/svg">
      <!-- backdrop -->
      <image v-if="visibility.showBackdrop" :href="images.backdropUrl" />

      <!-- activities -->
      <g v-if="visibility.showActions" v-for="(action, name, index) in images.actions" :key="name">
        <image :href="action.pictogram" :x="action.x" :y="action.y" :style="intensity(action.intensity)" />
        <text v-if="visibility.showLabels" :x="action.x" :y="action.y">
          <tspan>{{ name }}</tspan>
          <tspan dy="1.5em" :x="action.x" style="font-size: smaller">Freq.: {{ action.intensity }}</tspan>
        </text>
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'Tiramisu',
  data: () => ({
    dimensions: {
      width: 800,
      height: 508
    },
    images: {
      backdropUrl: 'https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/backdrop.png',
      actions: {
        "entrance": { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/entrance.png", x:732,y:228, intensity: 1 },
        "living":   { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/living.png",   x:181,y:305, intensity: 0 },
        "bedroom":  { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/bedroom.png",  x:535,y:338, intensity: 1 },
        "bathroom": { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/bathroom.png", x:549,y:14,  intensity: 0 },
        "kitchen":  { pictogram:"https://raw.githubusercontent.com/delas/tiramisu/master/examples/example2/kitchen.png",  x:171,y:14,  intensity: 0 }
      }
    },
    dfg: "https://raw.githubusercontent.com/delas/tiramisu/master/examples/example1.dfg",
    visibility: {
      showBackdrop: true,
      showActions: true,
      showLabels: false
    }
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
      for (const [key, value] of Object.entries(this.images.actions)) {
        const activity_name = key;
        const activity = value;
        const activity_frequency = Math.max(activity_to_frequency[activity_name], activity_from_frequency[activity_name]);
        activity.intensity = activity_frequency / most_frequent_activity;
      }
    },
    intensity(intensity) {
      return 'filter: drop-shadow(0px 0px '+ (intensity*20) +'px green)';
    }
  },
  mounted() {
    this.parseDFG(this.dfg);
  }
}
</script>