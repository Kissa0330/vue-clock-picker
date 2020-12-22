<template>
  <div class="time-picker-container">
    <div
      class="time-picker-preview"
      @click="toggleFocus"
      :class="{ active: focused }"
      @mouseover="overElement = true"
      @mouseout="overElement = false"
    >
      <div class="preview-container">{{ hourString }}:{{ minuteString }}</div>
    </div>
    <outside-click-handler
      :on-outside-click="onClearFocus"
      :focused="focused"
      v-if="focused"
    >
      <template #slotName>
        <time-picker-modal
          :init-hour="hour"
          :init-minute="minute"
          :handle-hour-change="handleHourChange"
          :handle-minute-change="handleMinuteChange"
          :handle-time-change="handleTimeChange"
          v-if="focused"
        />
      </template>
    </outside-click-handler>
  </div>
</template>

<script>
import OutsideClickHandler from "./OutsideClickHandler";
import TimePickerModal from "./TimePickerModal";
export default {
  components: {
    OutsideClickHandler,
    TimePickerModal,
  },
  props: {
    mode: {
      // AMかPMかを判定
      type: [String, Number],
      default: "24",
      validator: (value) => value == 24 || value == 12,
    },
    defaultHour: {
      // 初期設定時間
      type: [String, Number],
      default: new Date().getHours(),
      validator: (value) => value >= 0 && value < 24,
    },
    defaultMinute: {
      // 初期設定分
      type: [String, Number],
      default: new Date().getMinutes(),
      validator: (value) => value >= 0 && value < 60,
    },
    defaultFocused: {
      type: Boolean,
      default: false,
    },
    onFocusChange: {
      type: Function,
      default: () => {},
    },
    onHourChange: {
      type: Function,
      default: () => {},
    },
    onMinuteChange: {
      type: Function,
      default: () => {},
    },
    onTimeChange: {
      type: Function,
      default: () => {},
    }
  },
  data() {
    return {
      hour: this.defaultHour,
      minute: this.defaultMinute,
      // pickerの表示/非表示の判定
      focused: this.defaultFocused,
      // mouseoverを検知してスタイルを変更する
      overElement: false,
    };
  },
  computed: {
    hourString() {
      // 時間が一桁なら先頭に0を付ける+文字化する
      return this.hour < 10 ? "0" + this.hour : this.hour + "";
    },
    minuteString() {
      // 分が一桁なら先頭に0を付ける+文字化する
      return this.minute < 10 ? "0" + this.minute : this.minute + "";
    },
  },
  methods: {
    onFocus() {
      this.focused = true;
      this.onFocusChange && this.onFocusChange(true);
    },
    onClearFocus() {
      !this.overElement && (this.focused = false);
      !this.overElement && this.onFocusChange && this.onFocusChange(false);
    },
    toggleFocus() {
      this.focused = !this.focused;
      this.onFocusChange && this.onFocusChange(this.focused);
    },
    // handle...系統は全てtime-picker-modelに関数として渡されている
    handleHourChange(hour) {
      this.hour = hour;
      this.onHourChange && this.onHourChange(this.hour);
      this.handleTimeChange({
        hour: this.hour,
      });
    },
    handleMinuteChange(minute) {
      this.minute = minute;
      this.onMinuteChange && this.onMinuteChange(this.minute);
      this.handleTimeChange({
        minute: this.minute,
      });
    },
    handleTimeChange(timeObj) {
      this.hour = timeObj["hour"] ? timeObj["hour"] : this.hour;
      this.minute = timeObj["minute"] ? timeObj["minute"] : this.minute;
      this.onTimeChange &&
        this.onTimeChange({
          hour: this.hour,
          minute: this.minute,
        });
    },
  }
};
</script>

<style lang="css">
.time-picker-container {
  position: relative;
}

.time-picker-preview {
  position: relative;
  cursor: pointer;
  height: 50px;
  width: 150px;
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
