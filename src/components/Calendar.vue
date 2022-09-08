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
                scrollTime: '12:00',
                aspectRatio: 1.5,
                // initialEvents: INITIAL_EVENTS,
                selectable: true,
                height: 500,
                selectMirror: true,
                select: this.handleDateSelect,
                eventClick: this.handleEventClick,
                editable: true,
                resourceAreaHeaderContent: 'không biết',
                resources: 'https://fullcalendar.io/api/demo-feeds/resources.json?with-nesting&with-colors',
                events: 'https://fullcalendar.io/api/demo-feeds/events.json?single-day&for-resource-timeline',
            },
            currentEvents:[]
        }
    },
    methods: {
        
        
        handleDateSelect(selectInfo) {
            let title = prompt('có thêm được đâu nà ní')
            let calendarApi = selectInfo.view.calendar
            calendarApi.unselect() // clear date selection
            if (title) {
                    calendarApi.addEvent({
                    id: createEventId(),
                    title,
                    start: selectInfo.startStr,
                    end: selectInfo.endStr,
                    allTime: selectInfo.allday
                })
            }
        },
        handleEventClick(clickInfo) {
            if(confirm(`'xoa di ngai gi' '${clickInfo.event.title}'`)){
                clickInfo.event.remove()
            }
        },
       
    }
}
</script>
<style lang="">
    
</style>