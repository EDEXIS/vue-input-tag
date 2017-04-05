# vue-input-tag
## Forked From
[matiastucci/vue-input-tag](https://github.com/matiastucci/vue-input-tag)

> A Vue.js 2.0 input tag component inspired in [react-tagsinput](https://github.com/olahol/react-tagsinput) 

<p align="center">
  <img src="demo.gif" width="750" alt="Logo"/>
</p>

## Installation

``` bash
npm install vue-input-tag --save
```

and in your component:

``` javascript
import InputTag from 'vue-input-tag'
```

## Usage

``` html
  <input-tag
    :attachedTags="tagsFiltered"
    :allTags="tags"
    placeholder="Add Tags"
    v-on:newTag="create($event)"
    v-on:addTag="attach($event)"
    v-on:removeTag="detach($event)"
    class="col-md-12"
    style="display:inline-block; white-space: nowrap;"
  > </input-tag>
```

## Props
| Name | Type | Default | Description |
| ---:| --- | ---| --- |
| attachedTags | Array | [] | Tags to be render in the input |
| allTags | Array | [] | Tags to be compared with user input for search |
| placeholder | String | "" | Placeholder to be shown when no tags |
| read-only | Boolean | false | Set input to readonly |
| validate | String | "" | Apply certain validator for user input. Choose from `email`, `url`, `text`, `digits` or `isodate`

## Emits
| Name | Return | Description |
| ---:| --- | --- |
| addTag | String | A user added tag that needs to be added to "attachedTags" |
| removeTag | String | A tag that the user wants to remove from "attachedTags" |
| createTag | String | User created tag that isn't in the "allTags" Array|

