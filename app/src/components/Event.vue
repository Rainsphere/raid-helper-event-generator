<template>
  <div class="event">
    <div
      class="event__header"
      :class="{open: isOpen}"
      @click="isOpen = !isOpen"
    >
      <span class="event__title">
        <span v-if="eventTitle !== ''">{{ eventTitle }}</span>
        <span v-else>Untitled Event</span>
      </span>
      <span class="event__date">{{ eventDate }}<span v-if="eventTime !== ''"> at {{eventTime}}</span></span>
      <span class="event__header-arrow"></span>
    </div>
    <div
      class="event__body"
      :class="{open: isOpen}"
    >
      <div class="input_group">
        <label>Event Title</label>
        <input
          type="text"
          :value="eventTitle"
          @keyup="updateEventTitle"
        />
      </div>
      <div class="input_group">
        <label>Event Date</label>
        <input
          type="date"
          :value="eventDate"
          @keyup="updateEventDate"
          @change="updateEventDate"
        />
      </div>

      <div class="input_group">
        <label>Event Time</label>
        <input
          type="time"
          :value="eventTime"
          @keyup="updateEventTime"
          @change="updateEventTime"
        />
      </div>
      <div class="input_group">
        <label>Description</label>
        <textarea
          :value="eventDescription"
          @keyup="updateEventDescription"
        >
          </textarea>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, toRefs } from "vue";
export default {
  name: "Event",
  props: {
    event: {
      type: Object,
      required: true,
    },
  },
  setup(props: any, { emit }) {
    const { event } = toRefs(props);
    const eventTitle = ref(event.value.title);
    const eventDate = ref(event.value.date);
    const eventTime = ref(event.value.time);
    const eventDescription = ref(event.value.description);
    const isOpen = ref(false);
    const updateEvent = (): void => {
      console.log("updateEvent", eventTitle.value, eventDate.value);
      emit("updateEvent", {
        title: eventTitle.value,
        date: eventDate.value,
        time: eventTime.value,
        description: eventDescription.value,
        copied: false,
      });
    };
    const updateEventTitle = (e: any): void => {
      eventTitle.value = e.target.value;
      updateEvent();
    };
    const updateEventDate = (e: any): void => {
      eventDate.value = e.target.value;
      updateEvent();
    };

    const updateEventTime = (e: any): void => {
      console.log(e.target.value);
      eventTime.value = e.target.value;
      updateEvent();
    };

    const updateEventDescription = (e: any): void => {
      eventDescription.value = e.target.value;
      updateEvent();
    };

    return {
      eventTitle,
      eventDate,
      eventTime,
      eventDescription,
      updateEvent,
      updateEventTitle,
      updateEventDate,
      updateEventTime,
      updateEventDescription,
      isOpen,
    };
  },
};
</script>

<style scoped>
.event {
  background: #393e46;

  max-width: 890px;
  margin: 0 auto;
  min-height: 50px;
  border-radius: 6px;
  margin: 15px 0px;
}
.event__header {
  height: 60px;
  user-select: none;
  position: relative;
  color: #eeeeee;

  padding: 10px 60px 10px 20px;
  display: grid;
  grid-template-rows: 1fr 1fr;
}
.event__title {
  font-size: 18px;
  font-weight: 600;
  color: #eeeeee;
}
.event__date {
  font-size: 12px;
}
.event__header.open .event__header-arrow {
  transform: rotate(-180deg) translateY(50%);
}
.event__header-arrow {
  position: absolute;
  right: 28px;
  top: 40px;
  transform: translateY(-50%);
  transition: all 200ms linear;
}
.event__header-arrow:after {
  content: "";
  display: inline-block;
  width: 2px;
  height: 15px;
  background: #eeeeee;
  transform-origin: bottom;
  transform: rotate(45deg) translateX(-2px);
}
.event__header-arrow:before {
  content: "";
  display: inline-block;
  width: 2px;
  height: 15px;
  background: #eeeeee;
  transform-origin: bottom;
  transform: rotate(-45deg) translateX(2px);
}
.event__body {
  height: 0px;
  overflow: hidden;
  background: #393e46;
  transition: height 200ms linear;
  border-bottom-left-radius: 6px;
  border-bottom-right-radius: 6px;
}

.event__body.open {
  height: auto;
  padding: 20px;
  position: relative;
}
.event__body.open:after {
  content: "";
  display: block;
  width: calc(100% - 40px);
  height: 1px;
  background: #eeeeee;
  top: 0px;
  left: 20px;
  position: absolute;
}
.input_group {
  display: grid;
  grid-template-columns: 150px 1fr;
  margin-bottom: 20px;
}
label {
  font-weight: 500;
  color: #eeeeee;
}
input[type="text"],
input[type="date"],
input[type="time"] {
  border: none;
  border-radius: 6px;
  min-height: 40px;
  text-indent: 10px;
  outline: none;
}
textarea {
  border: none;
  border-radius: 6px;
  min-height: 80px;
  text-indent: 10px;
  outline: none;
  width: 100%;
  resize: none;
}
</style>