<template>
    <div class='demo-app'>
    
    <div class='demo-app-main'>
       <Fullcalendar 
            class='demo-app-calendar'
            :options="calendarPlugins"
        > 
      </FullCalendar>
    </div>
  </div>
</template>
<script>
import Fullcalendar from "@fullcalendar/vue";
import DayGridPlugin from "@fullcalendar/daygrid";
import TimeGridPlugin from "@fullcalendar/timegrid";
import InteractionPlugin from "@fullcalendar/interaction";
import ListPlugin from "@fullcalendar/list";
import resourceTimelineDayPlugin from "@fullcalendar/resource-timeline";
import { createEventId } from "../event/event-utils";
import { mapActions } from "vuex";
export default {
    name: "CalenDar",
    components: {
        Fullcalendar
    },
    data() {
        return {
            calendarPlugins: {
                plugins : [
                DayGridPlugin,
                TimeGridPlugin,
                InteractionPlugin,
                ListPlugin,
                resourceTimelineDayPlugin
                ],
                headerToolbar: { 
                    right: 'resourceTimelineDay,resourceTimelineWeek,resourceTimelineMonth', 
                    center: 'title',  
                    left:'prev,next', 
                },
                initialView: 'resourceTimelineDay',
                scrollTime: '08:00',
                aspectRatio: 1.5,
                // initialEvents: INITIAL_EVENTS,
                selectable: true,
                height: 500,
                dayMaxEvents: true,
                weekends: false,
                selectMirror: true,
                select: this.handleDateSelect,
                eventClick: this.handleEventClick,
                eventsSet: this.handleEvents,
                editable: true,
                resourceAreaHeaderContent: 'không biết',
                resources: 'https://fullcalendar.io/api/demo-feeds/resources.json?with-nesting&with-colors',
                events: 'https://fullcalendar.io/api/demo-feeds/events.json?single-day&for-resource-timeline',
                droppable: true,
               
            },
            currentEvents:[]
        }
    },
    methods: {
        ...mapActions([
            'updateEvent'
        ]),
        handleWeekendsToggle() {
             this.calendarPlugins.weekends = !this.calendarPlugins.weekends // update a property
         },

        handleDateSelect(selectInfo) {
            let title = prompt('Please enter a new title for your event')
            let calendarApi = selectInfo.view.calendar
            calendarApi.unselect() // clear date selection
            if (title) {
                    calendarApi.addEvent({
                    id: createEventId(),
                    title,
                    start: selectInfo.start,
                    end: selectInfo.end,
                    allTime: selectInfo.allTime
                })
            }
        },
        handleEventClick(clickInfo) {
            if(confirm(`'xoa di dung ngai' '${clickInfo.event.title}'`)){
                clickInfo.event.remove()
            }
        },
        handleEvents(events) {
            this.currentEvents = events
        },
        onEventDrop({event}){
            return this.updateEvent(event)
        }

    

    }
}
</script>
<style lang="">
    
</style>