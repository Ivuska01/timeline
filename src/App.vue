<template>
  <div class="timeline">
    <div
      v-for="(item, i) in timelineData"
      v-bind:key="i"
      :class='i % 2 ? "timeline-item left" : "timeline-item right"'
      
    >
      <div class="content">
        <h4>{{item.registered}}</h4>
        <h5>{{item.name.first}} {{item.name.last}}</h5>
        <p>{{item.about}}</p>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  name: 'app',
  data() {
    return {
      timelineData:[]
    }
  },

  async created() {
    try {
      const res = await axios.get(`http://localhost:3000/timelineData`);
      this.timelineData = res.data;
      this.timelineData = this.timelineData.sort(function(item,i) {
        return Date.parse(new Date(item.registered)) - Date.parse(new Date(i.registered));
      })
    } catch(e) {
      console.error(e);
    }
  }
};

</script>


<style>
body {
  font-family: sans-serif;
}

* {
  box-sizing: border-box;
}

.timeline {
  position: relative;
  max-width: 1200px;
  margin: 0 auto;
}

.timeline::after {
  content: "";
  position: absolute;
  width: 3px;
  background-color: rgb(150, 225, 238);
  top: 0;
  bottom: 0;
  left: 50%;
  margin-left: -2px;
}

.timeline-item {
  padding: 10px 40px;
  position: relative;
  background-color: inherit;
  width: 50%;
}

.timeline-item::after {
  content: "";
  position: absolute;
  width: 15px;
  height: 15px;
  right: -9px;
  background-color: #f10da5;
  border: 2px solid rgb(7, 247, 235);
  top: 22px;
  border-radius: 50%;
  z-index: 1;
}

.left {
  left: 0;
}

.right {
  left: 50%;
}

.left::before {
  content: " ";
  height: 0;
  position: absolute;
  top: 22px;
  width: 0;
  z-index: 1;
  right: 10px;
  border: medium solid rgb(90, 222, 226);
  border-width: 10px 0 10px 10px;
  border-color: transparent rgb(86, 226, 236);
}

.right::before {
  content: " ";
  height: 0;
  position: absolute;
  top: 22px;
  width: 0;
  z-index: 1;
  left: 10px;
  border: medium solid rgb(216, 195, 224);
  border-width: 10px 10px 10px 0;
  border-color: transparent rgb(153, 69, 202);
}

.right::after {
  left: -10px;
}

.content {
  padding: 10px;
  background-color: #f6d5f8;
  position: relative;
}

/* Responsive on screens less than 600px wide */
@media screen and (max-width: 600px) {
  .timeline::after {
    left: 10px;
  }

  .timeline-item {
    width: 100%;
    padding-left: 5px;
    padding-right: 5px;
  }

  .timeline-item::before {
    left: 5px;
    border: medium solid white;
    border-width: 10px 10px 10px 0;
    border-color: transparent white transparent transparent;
  }

  .left::after,
  .right::after {
    left: 5px;
  }

  .right {
    left: 0%;
  }
}
</style>
