<template>
  <div class="content">
    <h1>Events</h1>
    <p>Add events here to generate their commands.</p>
    <p v-if="events.length <= 0">Click "Add Event" to get started.</p>
    <div class="events">
      <event
        v-for="(event, key) in events"
        :key="key"
        :event="event"
        @updateEvent="updateEvent($event, key)"
      />
    </div>
    <div>
      <button
        class="add_button"
        @click="onAddEvent"
      >Add Event</button>
    </div>
    <div v-if="events.length > 0">
      <h1>Commands</h1>
      <p>Copy each command line below and paste it into the channel where you want to create your event.</p>
    </div>
    <div
      v-for="(event, key) in events"
      :key="'code' + key"
      class="code_block"
    >
      <pre class="eventCode">{{getCode(event)}}</pre>
      <button
        @click="copyToClipboard(key)"
        class="copy_button"
      >
        <span
          :class="{'copied': event.copied}"
          class="copy"
        ></span>
      </button>
    </div>
  </div>

</template>

<script lang="ts">
import Event from "./components/Event.vue";
import { ref, computed } from "vue";
export default {
  components: {
    Event,
  },
  setup() {
    const events = ref([]);
    const onAddEvent = () => {
      events.value.push({ title: "", date: "", time: "", description: "" });
    };
    const updateEvent = (event: any, key: any) => {
      console.log(event, key);
      events.value[key] = event;
    };

    const eventCode = computed(() => {
      const eventCodeValues = [];
      events.value.forEach((event: any) => {
        const command = `/quickcreate arguments: [template: 4][title: ${event.title}][description: ${event.description}][date: ${event.date}][time: ${event.time}]`;
        eventCodeValues.push(command);
      });
      return eventCodeValues;
    });

    const getCode = (event: any) => {
      const command = `/quickcreate arguments: [template: 4][title: ${event.title}][description: ${event.description}][date: ${event.date}][time: ${event.time}]`;
      return command;
    };

    const copyToClipboard = (key) => {
      // atlasImport.value.select();
      // atlasImport.value.setSelectionRange(0, 99999);
      navigator.clipboard.writeText(eventCode.value[key]);
      events.value[key].copied = true;
      setTimeout(() => {
        events.value[key].copied = false;
      }, 1500);
    };

    return {
      events,
      onAddEvent,
      eventCode,
      updateEvent,
      copyToClipboard,
      getCode,
    };
  },
};
</script>



<style>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;500;700&display=swap");
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css");

p {
  margin: 0px 10px;
}
.content {
  display: flex;
  flex-flow: column wrap;
  max-width: 890px;
  margin: 0 auto;
  gap: 20px;
}
.add_button {
  background: #00adb5;
  border: 0px;
  margin: 0 auto;
  display: block;
  height: 40px;
  padding: 0px 20px;
  border-radius: 6px;
  outline: none;
  font-weight: 600;
  transition: all 200ms linear;
  cursor: pointer;
  color: #222831;
}
.add_button:hover {
  background: #393e46;
  color: #eeeeee;
}
body {
  background: #eeeeee;
  margin: 0px;
  font-family: "Open Sans", sans-serif;
}
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.code_block {
  display: grid;
  grid-template-columns: 1fr 40px;
  gap: 20px;
  align-items: center;
}
pre {
  background: #222831;
  color: #eeeeee;
  padding: 10px;
  overflow: auto;
  max-width: 890px;
}
.copy_button {
  background: #00adb5;
  border: 0px;
  margin: 0 auto;
  display: block;
  height: 40px;

  border-radius: 6px;
  outline: none;
  font-weight: 600;
  transition: all 200ms linear;
  cursor: pointer;
  color: #222831;
  position: relative;
  font-size: 18px;
  width: 100%;
}
.copy_button:hover {
  background: #393e46;
  color: #eeeeee;
}

.copy_button .copy:after {
  content: "\f0c5";
  font-family: FontAwesome;
}
.copy_button .copy:before {
  opacity: 0;
  pointer-events: none;
  content: "Copied!";
  position: absolute;
  top: 0px;
  left: 50%;
  transform: translate(-50%, -100%);
  font-size: 14px;
  transition: opacity 200ms linear;
}
.copy_button .copy.copied:before {
  opacity: 1;
}
.copy_button .copy.copied:after {
  content: "\f00c";
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
