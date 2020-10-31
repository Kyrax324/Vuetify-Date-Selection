# vuetify-date-selection

- A Simple date input selection with two date-picker based on Vuetify.
- Return array of date

## Installation

```bash
npm install vuetify-date-selection --save
```

* [Demo](https://bbitwolf.github.io/Vue-Project-Demo/vuetify-date-selection)

## Usage


***any.vue***

```html
<v-date-selection v-model="date_input"></v-date-selection>
```

```js
import VDateSelection from 'vuetify-date-selection'
...
components:{ VDateSelection },
...
```

## Props & Slot

* Props

|Name|Type|Default|Description|
|-|-|-|-|
|range|Boolean|false|IsRange Mode
|start-text|String|"Start"|Show on start date is not selected
|end-text|String|"End"|Show on end date is not selected

* Props from Vuetify Date-picker  	[*(Refer Vuetify Documentation)*](https://vuetifyjs.com/en/components/date-pickers)

|Name|
|-|
|color
|event-color
|locale
|dark
|light
|first-day-of-week
|header-color

* Slot

|Slot|Description|
|-|-|
|default|Displayed below the selection, can be used for example for adding action button (OK and Cancel)|
|divider|Displayed between two date-pickers|
