<template>
    <my-page title="Cron 表达式生成" :page="page">
        <div class="editor">
            <div class="result" v-if="result">
                <span class="minute" :style="active2('minute')" title="分钟">{{ result.minute }}</span>
                <span class="hour" :style="active2('hour')" title="小时">{{ result.hour }}</span>
                <span class="day" :style="active2('day')" title="日">{{ result.day }}</span>
                <span class="month" :style="active2('month')"  title="月">{{ result.month }}</span>
                <span class="week" :style="active2('week')" title="星期">{{ result.week }}</span>
                <span class="year" :style="active2('year')" title="年">{{ result.year }}</span>
            </div>
            <ui-tabs :value="activeTab" @change="handleTabChange">
                <ui-tab value="minute" title="分钟"/>
                <ui-tab value="hour" title="小时"/>
                <ui-tab value="day" title="日"/>
                <ui-tab value="month" title="月"/>
                <ui-tab value="week" title="星期"/>
                <ui-tab value="year" title="年"/>
            </ui-tabs>
            <div v-if="activeTab === 'minute'">
                <div class="option-box">
                    <div class="item" :style="active('minute', 1)">
                        <ui-radio class="radio" v-model="options.minute.type" label="任意" name="group" :nativeValue="1"/>
                    </div>
                    <div class="item" :style="active('minute', 2)">
                        <ui-radio class="radio" v-model="options.minute.type" label="" name="group" :nativeValue="2"/>
                        第
                        <ui-text-field class="input" type="number" v-model="options.minute.start" />
                        到
                        <ui-text-field class="input" type="number" v-model="options.minute.end" />
                        分钟
                    </div>
                    <div class="item" :style="active('minute', 3)">
                        <ui-radio class="radio" v-model="options.minute.type" label="" name="group" :nativeValue="3"/>
                        从 
                        <ui-text-field class="input" type="number" v-model="options.minute.start2" />
                        分钟开始，每  
                        <ui-text-field class="input" type="number" v-model="options.minute.every" />
                        分钟执行一次
                    </div>
                    <div class="item" :style="active('minute', 4)">
                        <ui-radio class="radio" v-model="options.minute.type" label="" name="group" :nativeValue="4"/>
                        指定
                        <div class="list-box">
                            <ui-checkbox class="checkbox2" v-model="options.minute.list" 
                                v-for="n in 60"
                                :key="n"
                                name="group" :nativeValue="n - 1" :label="'' + (n - 1)" />
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="activeTab === 'hour'">
                <div class="option-box">
                    <div class="item" :style="active('hour', 1)">
                        <ui-radio class="radio" v-model="options.hour.type" label="任意" name="group" :nativeValue="1"/>
                    </div>
                    <div class="item" :style="active('hour', 2)">
                        <ui-radio class="radio" v-model="options.hour.type" label="" name="group" :nativeValue="2"/>
                        第
                        <ui-text-field class="input" type="number" v-model="options.hour.start" />
                        到
                        <ui-text-field class="input" type="number" v-model="options.hour.end" />
                        小时
                    </div>
                    <div class="item" :style="active('hour', 3)">
                        <ui-radio class="radio" v-model="options.hour.type" label="" name="group" :nativeValue="3"/>
                        从 
                        <ui-text-field class="input" type="number" v-model="options.hour.start2" />
                        小时开始，每  
                        <ui-text-field class="input" type="number" v-model="options.hour.every" />
                        小时执行一次
                    </div>
                    <div class="item" :style="active('hour', 4)">
                        <ui-radio class="radio" v-model="options.hour.type" label="" name="group" :nativeValue="4"/>
                        指定
                        <div class="list-box">
                            <ui-checkbox class="checkbox2" v-model="options.hour.list" 
                                v-for="n in 24"
                                :key="n"
                                name="group" :nativeValue="n - 1" :label="'' + (n - 1)" />
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="activeTab === 'day'">
                <div class="option-box">
                    <div class="item" :style="active('day', 1)">
                        <ui-radio class="radio" v-model="options.day.type" label="任意" name="group" :nativeValue="1"/>
                    </div>
                    <div class="item" :style="active('day', 5)">
                        <ui-radio class="radio" v-model="options.day.type" label="不指定" name="group" :nativeValue="5"/>
                    </div>
                    <div class="item" :style="active('day', 2)">
                        <ui-radio class="radio" v-model="options.day.type" label="" name="group" :nativeValue="2"/>
                        第
                        <ui-text-field class="input" type="number" v-model="options.day.start" />
                        到
                        <ui-text-field class="input" type="number" v-model="options.day.end" />
                        日
                    </div>
                    <div class="item" :style="active('day', 3)">
                        <ui-radio class="radio" v-model="options.day.type" label="" name="group" :nativeValue="3"/>
                        从 
                        <ui-text-field class="input" type="number" v-model="options.day.start2" />
                        日开始，每  
                        <ui-text-field class="input" type="number" v-model="options.day.every" />
                        日执行一次
                    </div>
                    <div class="item" :style="active('day', 7)">
                        <ui-radio class="radio" v-model="options.day.type" label="" name="group" :nativeValue="7"/>
                        每月
                        <ui-text-field class="input" type="number" v-model="options.day.latest" />
                        号最近的那个工作日
                    </div>
                    <div class="item" :style="active('day', 6)">
                        <ui-radio class="radio" v-model="options.day.type" label="本月最后一天" name="group" :nativeValue="6"/>
                    </div>
                    <div class="item" :style="active('day', 4)">
                        <ui-radio class="radio" v-model="options.day.type" label="" name="group" :nativeValue="4"/>
                        指定
                        <div class="list-box">
                            <ui-checkbox class="checkbox2" v-model="options.day.list" 
                                v-for="n in 31"
                                :key="n"
                                name="group" :nativeValue="n" :label="n" />
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="activeTab === 'month'">
                <div class="option-box">
                    <div class="item" :style="active('month', 1)">
                        <ui-radio class="radio" v-model="options.month.type" label="任意" name="group" :nativeValue="1"/>
                    </div>
                    <div class="item" :style="active('month', 5)">
                        <ui-radio class="radio" v-model="options.month.type" label="不指定" name="group" :nativeValue="5"/>
                    </div>
                    <div class="item" :style="active('month', 2)">
                        <ui-radio class="radio" v-model="options.month.type" label="" name="group" :nativeValue="2"/>
                        第
                        <ui-text-field class="input" type="number" v-model="options.month.start" />
                        到
                        <ui-text-field class="input" type="number" v-model="options.month.end" />
                        月
                    </div>
                    <div class="item" :style="active('month', 3)">
                        <ui-radio class="radio" v-model="options.month.type" label="" name="group" :nativeValue="3"/>
                        从 
                        <ui-text-field class="input" type="number" v-model="options.month.start2" />
                        月开始，每  
                        <ui-text-field class="input" type="number" v-model="options.month.every" />
                        月执行一次
                    </div>
                    <div class="item" :style="active('month', 4)">
                        <ui-radio class="radio" v-model="options.month.type" label="" name="group" :nativeValue="4"/>
                        指定
                        <div class="list-box">
                            <ui-checkbox class="checkbox2" v-model="options.month.list" 
                                v-for="n in 12"
                                :key="n"
                                name="group" :nativeValue="n" :label="n" />
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="activeTab === 'week'">
                <div class="option-box">
                    <div class="item" :style="active('week', 1)">
                        <ui-radio class="radio" v-model="options.week.type" label="任意" name="group" :nativeValue="1"/>
                    </div>
                    <div class="item" :style="active('week', 5)">
                        <ui-radio class="radio" v-model="options.week.type" label="不指定" name="group" :nativeValue="5"/>
                    </div>
                    <div class="item" :style="active('week', 2)">
                        <ui-radio class="radio" v-model="options.week.type" label="" name="group" :nativeValue="2"/>
                        第
                        <ui-text-field class="input" type="number" v-model="options.week.start" />
                        到
                        <ui-text-field class="input" type="number" v-model="options.week.end" />
                        星期
                    </div>
                    <!-- <div class="item">
                        <ui-radio class="radio" v-model="options.week.type" label="" name="group" :nativeValue="3"/>
                        从 
                        <ui-text-field class="input" type="number" v-model="options.week.start2" />
                        分钟开始, 每  
                        <ui-text-field class="input" type="number" v-model="options.week.every" />
                        分钟执行一次
                    </div> -->
                    <div class="item" :style="active('week', 7)">
                        <ui-radio class="radio" v-model="options.week.type" label="" name="group" :nativeValue="7"/>
                        第
                        <ui-text-field class="input" type="number" v-model="options.week.index" />
                        周的星期
                        <ui-text-field class="input" type="number" v-model="options.week.index2" />
                    </div>
                    <div class="item" :style="active('week', 6)">
                        <ui-radio class="radio" v-model="options.week.type" label="" name="group" :nativeValue="6"/>
                        本月最后一个星期
                        <ui-text-field class="input" type="number" v-model="options.week.latest" />
                    </div>
                    <div class="item" :style="active('week', 4)">
                        <ui-radio class="radio" v-model="options.week.type" label="" name="group" :nativeValue="4"/>
                        指定
                        <div class="list-box">
                            <ui-checkbox class="checkbox2" v-model="options.week.list" 
                                v-for="n in 7"
                                :key="n"
                                name="group" :nativeValue="n" :label="n" />
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="activeTab === 'year'">
                <div class="option-box">
                    <div class="item" :style="active('year', 1)">
                        <ui-radio class="radio" v-model="options.year.type" label="任意" name="group" :nativeValue="1"/>
                    </div>
                    <div class="item" :style="active('year', 5)">
                        <ui-radio class="radio" v-model="options.year.type" label="不指定" name="group" :nativeValue="5"/>
                    </div>
                    <div class="item" :style="active('year', 2)">
                        <ui-radio class="radio" v-model="options.year.type" label="" name="group" :nativeValue="2"/>
                        第
                        <ui-text-field class="input" type="number" v-model="options.year.start" />
                        到
                        <ui-text-field class="input" type="number" v-model="options.year.end" />
                        年
                    </div>
                </div>
            </div>
        </div>
        <ui-appbar class="edit-box" v-if="editVisible">
            <ui-icon-button icon="close" slot="left" @click="editVisible = false" />
            <input class="input" v-model="input" slot="left" ref="input">
            <ui-icon-button icon="check" slot="right" @click="finish" />
        </ui-appbar>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
                input: '',
                result: '',
                options: {
                    minute: {
                        type: 1,
                        start: 1,
                        end: 2,
                        start2: 0,
                        every: 1,
                        list: [1]
                    },
                    hour: {
                        type: 1,
                        start: 1,
                        end: 2,
                        start2: 0,
                        every: 1,
                        list: [1]
                    },
                    day: {
                        type: 1,
                        start: 1,
                        end: 2,
                        start2: 0,
                        every: 1,
                        list: [1],
                        latest: 1
                    },
                    month: {
                        type: 1,
                        start: 1,
                        end: 2,
                        start2: 0,
                        every: 1,
                        list: [1],
                        latest: 1
                    },
                    week: {
                        type: 1,
                        start: 1,
                        end: 2,
                        start2: 0,
                        every: 1,
                        list: [1],
                        latest: 1,
                        index: 1,
                        index2: 1
                    },
                    year: {
                        type: 5,
                        start: 2018,
                        end: 2019
                    }
                },
                activeTab: 'minute',
                editVisible: false,
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'edit',
                            click: this.toggleEdit,
                            title: '直接编辑表达式'
                        }
                    ]
                }
            }
        },
        mounted() {
            this.gen()
            let data = this.$route.query.data
            if (data) {
                this.parser(data)
            }
            // this.parser('* * * * * 2019-2020')
        },
        methods: {
            toggleEdit() {
                this.input = this.result.text
                this.editVisible = true
                this.$nextTick(() => {
                    this.$refs.input.focus()
                })
            },
            finish() {
                this.editVisible = false
                this.parser(this.input)
            },
            handleTabChange (val) {
                this.activeTab = val
            },
            handleActive () {
                window.alert('tab active')
            },
            copyAndSort(arr) {
                let newArr = []
                for (let item of arr) {
                    newArr.push(item)
                }
                return newArr.sort()
            },
            gen() {
                let minuteText = ''
                let minute = this.options.minute
                if (minute.type === 1) {
                    minuteText = '*'
                } else if (minute.type === 2) {
                    minuteText = minute.start + '-' + minute.end
                } else if (minute.type === 3) {
                    minuteText = minute.start2 + '/' + minute.every
                } else if (minute.type === 4) {
                    minuteText = this.copyAndSort(minute.list).join(',')
                }

                let hourText = ''
                let hour = this.options.hour
                if (hour.type === 1) {
                    hourText = '*'
                } else if (hour.type === 2) {
                    hourText = hour.start + '-' + hour.end
                } else if (hour.type === 3) {
                    hourText = hour.start2 + '/' + hour.every
                } else if (hour.type === 4) {
                    hourText = this.copyAndSort(hour.list).join(',')
                }

                let dayText = ''
                let day = this.options.day
                if (day.type === 1) {
                    dayText = '*'
                } else if (day.type === 2) {
                    dayText = day.start + '-' + day.end
                } else if (day.type === 3) {
                    dayText = day.start2 + '/' + day.every
                } else if (day.type === 4) {
                    dayText = this.copyAndSort(day.list).join(',')
                } else if (day.type === 5) {
                    dayText = '?'
                } else if (day.type === 6) {
                    dayText = 'L'
                } else if (day.type === 7) {
                    dayText = day.latest + 'W'
                }

                let monthText = ''
                let month = this.options.month
                if (month.type === 1) {
                    monthText = '*'
                } else if (month.type === 2) {
                    monthText = month.start + '-' + month.end
                } else if (month.type === 3) {
                    monthText = month.start2 + '/' + month.every
                } else if (month.type === 4) {
                    monthText = this.copyAndSort(month.list).join(',')
                } else if (month.type === 5) {
                    monthText = '?'
                } else if (month.type === 6) {
                    monthText = 'L'
                } else if (month.type === 7) {
                    monthText = month.latest + 'W'
                }

                let weekText = ''
                let week = this.options.week
                if (week.type === 1) {
                    weekText = '*'
                } else if (week.type === 2) {
                    weekText = week.start + '-' + week.end
                } else if (week.type === 3) {
                    weekText = week.start2 + '/' + week.every
                } else if (week.type === 4) {
                    weekText = this.copyAndSort(week.list).join(',')
                } else if (week.type === 5) {
                    weekText = '?'
                } else if (week.type === 6) {
                    weekText = week.latest + 'L'
                } else if (week.type === 7) {
                    weekText = week.index + '#' + week.index2
                }

                let yearText = ''
                let year = this.options.year
                if (year.type === 1) {
                    yearText = '*'
                } else if (year.type === 2) {
                    yearText = year.start + '-' + year.end
                } else if (year.type === 3) {
                    // yearText = year.start2 + '/' + year.every
                } else if (year.type === 4) {
                    // yearText = this.copyAndSort(year.list).join(',')
                } else if (year.type === 5) {
                    yearText = ''
                }

                this.result = {
                    minute: minuteText,
                    hour: hourText,
                    day: dayText,
                    month: monthText,
                    week: weekText,
                    year: yearText,
                    text: `${minuteText} ${hourText} ${dayText} ${monthText} ${weekText} ${yearText}`
                }
            },
            active(type, type2) {
                if (this.options[type].type === type2) {
                    return {}
                }
                return {
                    opacity: 0.2
                }
            },
            active2(type) {
                if (this.activeTab === type) {
                    return {
                        color: '#f00'
                    }
                }
                return {}
            },
            parser(text) {
                let arr = text.split(' ')
                this.parserMinute(arr[0])
                this.parserHour(arr[1])
                this.parserDay(arr[2])
                this.parserMonth(arr[3])
                this.parserWeek(arr[4])
                if (arr.length > 5) {
                    this.parserYear(arr[5])
                }
            },
            parserMinute(text) {
                let minute = this.options.minute
                if (text === '*') {
                    minute.type = 1
                } else if (text.includes('-')) {
                    minute.type = 2
                    let arr = text.split('-')
                    minute.start = parseInt(arr[0])
                    minute.end = parseInt(arr[1])
                } else if (text.includes('/')) {
                    minute.type = 3
                    let arr = text.split('/')
                    minute.start2 = parseInt(arr[0])
                    minute.every = parseInt(arr[1])
                } else if (text.includes(',')) {
                    minute.type = 4
                    let arr = text.split(',')
                    minute.list = arr
                }
            },
            parserHour(text) {
                let hour = this.options.hour
                if (text === '*') {
                    hour.type = 1
                } else if (text.includes('-')) {
                    hour.type = 2
                    let arr = text.split('-')
                    hour.start = parseInt(arr[0])
                    hour.end = parseInt(arr[1])
                } else if (text.includes('/')) {
                    hour.type = 3
                    let arr = text.split('/')
                    hour.start2 = parseInt(arr[0])
                    hour.every = parseInt(arr[1])
                } else if (text.includes(',')) {
                    hour.type = 4
                    let arr = text.split(',')
                    hour.list = arr
                }
            },
            parserDay(text) {
                let day = this.options.day
                if (text === '*') {
                    day.type = 1
                } else if (text.includes('-')) {
                    day.type = 2
                    let arr = text.split('-')
                    day.start = parseInt(arr[0])
                    day.end = parseInt(arr[1])
                } else if (text.includes('/')) {
                    day.type = 3
                    let arr = text.split('/')
                    day.start2 = parseInt(arr[0])
                    day.every = parseInt(arr[1])
                } else if (text.includes(',')) {
                    day.type = 4
                    let arr = text.split(',')
                    day.list = arr
                } else if (text.includes('?')) {
                    day.type = 5
                } else if (text.includes('L')) {
                    day.type = 6
                } else if (text.includes('W')) {
                    day.type = 7
                    day.latest = parseInt(text.replace('W', ''))
                }
            },
            parserMonth(text) {
                let month = this.options.month
                if (text === '*') {
                    month.type = 1
                } else if (text.includes('-')) {
                    month.type = 2
                    let arr = text.split('-')
                    month.start = parseInt(arr[0])
                    month.end = parseInt(arr[1])
                } else if (text.includes('/')) {
                    month.type = 3
                    let arr = text.split('/')
                    month.start2 = parseInt(arr[0])
                    month.every = parseInt(arr[1])
                } else if (text.includes(',')) {
                    month.type = 4
                    let arr = text.split(',')
                    month.list = arr
                } else if (text.includes('?')) {
                    month.type = 5
                }
            },
            parserWeek(text) {
                let week = this.options.week
                if (text === '*') {
                    week.type = 1
                } else if (text.includes('-')) {
                    week.type = 2
                    let arr = text.split('-')
                    week.start = parseInt(arr[0])
                    week.end = parseInt(arr[1])
                } else if (text.includes('/')) {
                    // week.type = 3
                    // let arr = text.split('/')
                    // week.start2 = parseInt(arr[0])
                    // week.every = parseInt(arr[1])
                } else if (text.includes(',')) {
                    week.type = 4
                    let arr = text.split(',')
                    week.list = arr
                } else if (text.includes('?')) {
                    week.type = 5
                } else if (text.includes('L')) {
                    week.type = 6
                    week.latest = parseInt(text.replace('L', ''))
                } else if (text.includes('#')) {
                    week.type = 7
                    let arr = text.split('#')
                    week.index = parseInt(arr[0])
                    week.index2 = parseInt(arr[1])
                }
            },
            parserYear(text) {
                let year = this.options.year
                if (text === '*') {
                    year.type = 1
                } else if (text.includes('-')) {
                    year.type = 2
                    let arr = text.split('-')
                    year.start = parseInt(arr[0])
                    year.end = parseInt(arr[1])
                } else {
                    year.type = 5
                }
            }
        },
        watch: {
            options: {
                deep: true,
                handler() {
                    this.gen()
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
.edit-box {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 1000000;
    .input {
        border: none;
        color: #fff;
        background-color: transparent;
        outline: none;
    }
}
.editor {
    max-width: 600px;
}
.result {
    margin-bottom: 16px;
    font-size: 20px;
    text-align: center;
    span {
        padding-left: 4px;
        padding-right: 4px;
    }
}
.item {
    margin-bottom: 0px;
}
.radio {
    position: relative;
    top: 8px;
    margin-right: 8px;
}
.input {
    width: 80px;
    margin: 0 8px;
}
.list-box {
    margin-top: 8px;
    padding-left: 32px;
    max-width: 600px;
    .checkbox2 {
        width: 40px;
        margin-right: 16px;
    }
}
</style>
