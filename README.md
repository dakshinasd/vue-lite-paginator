# vue-lite-paginator
A lite weight VueJS paginator

### Installation
1. Download Paginator folder and put it inside your /src/component folder
1. Import it to your component by `import Paginator from "../Paginator";`
1. Register the components in components object `components: {Paginator}`
1. Place it on template wherever you want using `<Paginator />`

### Parameters/Events
Name | Description | Remarks
---- | ----------- | --------
:resource | An array of items to be filtered | required
:perPage | How many items should be displayed per page | required (default is 3)
:classes | Classes you want to add to the outside wrapper | optional
showPageNumber | Show/Hide Page numbers | optional (default true)
@updateResource | A custom event with filtered payload as a parameter | -
@resourceError | A custom event with resource error payload | -

### Usage
`
<Paginator 
  :resource = "rawData" 
  :perPage="3" 
  :showPageNumbers=true
  @updateResource="updateResource"
/>
`

### To Do
- [x] Props validation
- [ ] Better error handling
- [ ] CSS class binding for all the elements
- [ ] On demand elements show/hide
- [ ] Better documentation
- [ ] NPM Package and bundle as a VusJS plugin
- [ ] More features (?)
