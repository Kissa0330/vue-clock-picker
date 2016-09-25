<template lang="html">
	<div class="time-picker-container">
		<div class="time-picker-preview" @click="toggleFocus" :class="{active:focused}">
			<div class="preview-container">
				{{hourString}}:{{minuteString}}
			</div>
		</div>
		<outside-click-handler :on-outside-click="onClearFocus">
			<time-picker-modal
				:init-hour="hour" :init-minute="minute" slot="modal"
				:handle-hour-change="handleHourChange" :handle-minute-change="handleMinuteChange"
				:handle-time-change="handleTimeChange" v-if="focused"
			/>
		</outside-click-handler>
	</div>
</template>

<script>
import OutsideClickHandler from './OutsideClickHandler'
import TimePickerModal from './TimePickerModal'
export default {
	components: {
		OutsideClickHandler,
		TimePickerModal
	},
	props: {
		mode: {
			type: [String, Number],
			default: '24',
			validator: value => value == 24 || value == 12
		},
		defaultHour: {
			type: [String, Number],
			default: new Date().getHours(),
			validator: value => value >= 0 && value < 24
		},
		defaultMinute: {
			type: [String, Number],
			default: new Date().getMinutes(),
			validator: value => value >= 0 && value < 60
		},
		defaultFocused: {
			type: Boolean,
			default: false,
		},
		onFocusChange: {
			type: Function,
			default: () => {}
		},
		onHourChange: {
			type: Function,
			default: () => {}
		},
		onMinuteChange: {
			type: Function,
			default: () => {}
		},
		onTimeChange: {
			type: Function,
			default: () => {}
		}
	},
	data() {
		return {
			hour: this.defaultHour,
			minute: this.defaultMinute,
			focused: this.defaultFocused
		}
	},
	computed: {
		hourString() {
			return this.hour < 10 ? '0' + this.hour : this.hour
		},
		minuteString() {
			return this.minute < 10 ? '0' + this.minute : this.minute
		}
	},
	methods: {
		onFocus() {
			this.focused = true
			this.onFocusChanged && this.onFocusChanged(true)
		},
		onClearFocus() {
			this.focused = false
			this.onFocusChanged && this.onFocusChanged(false)
		},
		toggleFocus() {
			this.focused = !this.focused
			this.onFocusChanged && this.onFocusChanged(this.focused)
		},
		handleHourChange(hour) {
			this.hour = hour
			this.onHourChanged && this.onHourChanged(this.hour)
			this.handleTimeChange({
				hour: this.hour
			})
		},
		handleMinuteChange(minute) {
			this.minute = minute
			this.onMinuteChanged && this.onMinuteChanged(this.minute)
			this.handleTimeChange({
				minute: this.minute
			})
		},
		handleTimeChange(timeObj) {
			this.hour = timeObj['hour'] ? timeObj['hour'] : this.hour
			this.minute = timeObj['minute'] ? timeObj['minute'] : this.minute
			this.onTimeChanged && this.onTimeChanged({
				hour: this.hour,
				minute: this.minute
			})
		}
	}
}
</script>

<style lang="css">
.time-picker-container {
  position: relative;
}

.time-picker-preview {
	position: relative;
  	cursor: pointer;
  	height: 50px;
	width: 300px;
  	color: #343434;
  	border-radius: 2px;
  	background-color: #fff;
  	transition: all 400ms cubic-bezier(0.165, 0.84, 0.44, 1);
	box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.12), 0 0 4px 0 rgba(0, 0, 0, 0.08);
}

.time-picker-preview.active,
.time-picker-preview:hover {
  box-shadow: 0 8px 8px 0 rgba(0, 0, 0, 0.12), 0 0 8px 0 rgba(0, 0, 0, 0.08);
}

.time-picker-preview.active {
  color: #fff;
  background-color: #3498db;
}

.preview-container {
  position: absolute;
  top: 50%;
  left: 50%;
  height: 40px;
  line-height: 40px;
  /*padding-left: 30px;*/
  transform: translate(-50%, -50%);
  font-size: 2rem;
}
</style>