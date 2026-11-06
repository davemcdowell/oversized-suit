<h1 align="center">👔<br>Oversized Suit</h1>

An easy-to-use, super fast, client-side faceted search class built for static sites—or really, anywhere you have some JSON you need to search. Powered by vanilla JS and [MiniSearch](https://github.com/lucaong/minisearch). 

> <em>"Oversized and probably handles too much, but hey you can wear it how you want and it's got a lot of room to grow; plus it can be tailored!"</em>

> [!NOTE]
> This project has used no AI to generate any of the code provided. This is also a work in progress, so some features with URL state, UI auto-fill based on URL state and other aspects may not be fully implemented or will change over time 💖

## Why it's cool
* **Purely Static + Zero Cost:** No server, no database, no expensive monthly SaaS search bills.
* **Super Fast:** Fully client-side processing handles queries instantly.
* **Faceted Out-of-the-box:** Built with faceted search in mind from the start with default `AND/OR` filtering.
* **Pagination Ready:** Ready to render results with pagination (or not!)
* **Offline-First Ready:** Pairs perfectly with a service worker to cache your pre-built index.
* **Declarative UI:** Use your own markup and styling. Define key elements easily with query selectors, data attributes and templates.
* **Works How You Want:** Define your own search and template rendering logic or use the built in defaults.
* **State UR business:** Makes use of `URLSearchParams`, so searches can be bookmarked as well as auto-populated on page load.

## How to wear it

### The Markup
Oversized Suit (OsS) requires query selectors for two parent containers `searchContainer` and `resultsContainer` to narrow its selection down to elements within those containers. This method is also used for `paginationControls`. From here OsS makes use of data attributes to select elements like inputs, controls and outputs.

_Take a look at the example markup below to get an idea of how to structure or adapt your own._

```html
<!-- Search Funnel / Filters -->
<input type="checkbox" data-facet-toggle name="category" value="hotels" />
<button data-facet-toggle name="amenities" value="pool">Pool</button>

<!-- Custom Range -->
<double-range-slider data-range-name="price" min="0" max="1000"></double-range-slider>

<!-- Target Container -->
<div id="search-results"></div>
```

#### Data Attributes - Search Container
> [!NOTE]
> All attributes listed here are expected to exist within a parent HTML element defined by `searchContainer` in [Options](https://github.com/davemcdowell/OversizedSuit#class-options). Typically a `<search>` element will suffice.
> _i.e._ <code>&lt;search id="search-container"&gt;...&lt;/search&gt;</code>

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Responsibility</th>
      <th>Required Fields</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>data&#8209;search&#8209;input</code></td>
      <td>Defines the search text input. Must be placed on an <code>input</code> element of type <code>search</code> or <code>text</code>, other types <em>should</em> work as well depending on your JSON structure and <code>msFields</code> Search Settings used in <a href="https://github.com/davemcdowell/OversizedSuit#class-options">Options</a>.</td>
      <td><em>None</em></td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;input type="search" placeholder="Search" aria-label="Search" data-search-input&gt;</code></td>
    </tr>
    <tr>
      <td><code>data&#8209;clear&#8209;input</code></td>
      <td>
        Defines the element responsible for clearing the search input and resetting search results. Typically used on a <code>button</code> element.
        <br><br><strong>ℹ️ NOTE: </strong><em>Does not reset search filter toggles!</em>
      </td>
      <td><em>None</em></td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;button type="button" aria-label="Clear search input" data-clear-input&gt;</code></td>
    </tr>
    <tr>
      <td><code>data&#8209;search&#8209;filter</code></td>
      <td>Defines a search filter toggle. Must be placed on an <code>input</code> element of type <code>checkbox</code>.</td>
      <td><strong><code>name</code></strong> — Defines which <code>filterFields</code> array this toggle belongs to<br><br><strong><code>value</code></strong> — Defines the string to add to the active filters array defined in <code>name</code></td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;input type="checkbox" name="amenities" value="pool" data-search-filter&gt;</code></td>
    </tr>
    <tr>
      <td><code>data&#8209;clear&#8209;group&#8209;filters</code></td>
      <td>Defines an element used for clearing a given group of search filter toggles. Typically used on a <code>button</code> element.</td>
      <td><strong><code>name</code></strong> — Defines which <code>filterFields</code> array and search filters to clear and reset.</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;button type="button" name="type" data-clear-group-filters&gt;Clear Group&lt;/button&gt;</code></td>
    </tr>
    <tr>
      <td><code>data&#8209;clear&#8209;all&#8209;filters</code></td>
      <td>Defines an element used for clearing <em>all</em> search filter toggles and <code>filterFields</code> arrays. Typically used on a <code>button</code> element.</td>
      <td><em>None</em></td>
    </tr>
      <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;button type="button" data-clear-all-filters&gt;Clear Group&lt;/button&gt;</code></td>
    </tr>
  </tbody>
</table>

#### Data Attributes - Pagination Controls
> [!NOTE]
> All attributes listed here are expected to exist within a parent HTML element defined by `paginationControls` in [Options](https://github.com/davemcdowell/OversizedSuit#class-options). Typically a `<div>` element will suffice.
> _i.e._ <code>&lt;div id="page-controls"&gt;...&lt;/div&gt;</code>

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Responsibility</th>
      <th>Required Fields</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>data&#8209;page&#8209;previous</code></td>
      <td>Defines the previous page element for pagination controls. Typically used on a <code>button</code> element.</td>
      <td><em>None</em></td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;button aria-label="Previous page" data-page-previous&gt;Previous&lt;/button&gt;</code></td>
    </tr>
    <tr>
      <td><code>data&#8209;page&#8209;next</code></td>
      <td>Defines the next page element for pagination controls. Typically used on a <code>button</code> element.</td>
      <td><em>None</em></td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;button aria-label="Next page" data-page-next&gt;Next&lt;/button&gt;</code></td>
    </tr>
  </tbody>
</table>

## The Implementation
Instantiate the class, define required HTML elements and field arrays for filtering as well as any range slider object definitions. You can also set search configs for MiniSearch, pagination behavior and any custom search logic/rendering methods required for your use case. You can make use of JS modules, IIFEs or just a `DOMContentLoaded` event (example shown below), whatever you're more comfortable with as long as an instance of the class exist with your [Options](https://github.com/davemcdowell/OversizedSuit#class-options).

```javascript
document.addEventListener('DOMContentLoaded', () => {
    const mySearch = new OversizedSuit({
        //raw json search index to use with minisearch 
        searchData: '#search-data-raw',

        //the template for rendering results
        resultTemplate: '#results-template',
        
        //what filter fields make up active filters? (must match toggle group name fields)
        filterFields: { 'type': [], 'locations': [], 'amenities': [], 'tags': [], 'vibes': [] },

        //define any ranges used in this search
        ranges: [
            {
                rangeInput: "#price-range",
                rangeEvent: 'range-slider:change',
                rangeFields: { fromPrice: 0, toPrice: 0 },
                rangeInitiateMethod: initiatePriceRange,
                rangeEventMethod: priceChangeEvent,
                rangeFilterLogic: priceFilter
            }
        ],

        //search configs for minisearch
        searchConfig: {
            //minisearch: fields for minisearch to use for text searches (must match field names from yml/json)
            msFields: ['title', 'description'],

            //minisearch: return fields for minisearch results (must match filter fields defined above)
            msStoreFields: ['title', 'type', 'description', 'price', 'url', 'locations', 'amenities', 'tags', 'vibes'], 
            
            //minisearch: boost fields object to define field names and values for minisearch
            msBoostFields: { 'title': 2 },
        },

        //container selectors
        containers: {
            searchContainer: '#search-container',
            resultsContainer: '#results-container'
        },

        //pagination configs
        pagination: {
            paginationControls: '#page-controls',
            paginationProgressBar: '#pagination-progress',
            resultsPerPage: 10
        },

        //summary elements
        summary: {
            pageIndexSummary: '#index-summary',
            pageResultSummary: '#results-summary',
            noResultsFeedback: '#no-results'
        },

        //custom logic for search & rendering
        filterLogic: null,
        renderLogic: customTemplateRenderFunction
    });
});
```
#### General Options
<table>
  <thead>
    <tr>
      <th>Option Name</th>
      <th>Summary</th>
      <th>Required?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>searchData</code></td>
      <td>Query selector that points to the <code>script</code> element holding the JSON array of searchable objects <em>i.e. the raw search index</em>.
        <br><br><strong>ℹ️ NOTE: </strong><em>Async URL fetching coming soon!</em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;script id="search-data-raw" type="application/json&gt;...&lt;/script&gt;</code></td>
    </tr>
    <tr>
      <td><code>resultTemplate</code></td>
      <td>Query selector that points to the <code>template</code> containing a result HTML structure to be used when rendering search results.</td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;template id="results-template"&gt;...&lt;/template&gt;</code></td>
    </tr>
    <tr>
      <td><code>filterFields</code></td>
      <td>
        An object holding several (or one) key-values to define the name of corresponding JSON keys in the search index as active filter arrays in a given search instance.
        <br><br><strong>ℹ️ NOTE: </strong><em>Filter field names must match JSON keys exactly!</em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>{ 'type': [], 'locations': [], 'amenities': [], 'tags': [], 'vibes': [] }</code></td>
    </tr>
    <tr>
      <td><code>ranges</code></td>
      <td>
        An object array holding definitions for any range inputs to be used with this search. Supports custom double range sliders and normal <code>input</code> ranges.
        <br><br><strong>🔍 SEE: </strong><em><a href="https://github.com/davemcdowell/OversizedSuit/tree/main#range-options">Range Options</a> for more details on integrating various range sliders.</em>
      </td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <pre><code>
[
  {   
    rangeInput: "#price-range",
    rangeEvent: 'range-slider:change',
    rangeFields: { fromPrice: 0, toPrice: 0 },
    rangeInitiateMethod: initiatePriceRange,
    rangeEventMethod: priceChangeEvent,
    rangeFilterLogic: priceFilter 
  },
  ...
]</code></pre>
      </td>
    </tr>
    <tr>
      <td><code>searchConfig</code></td>
      <td>An object to configure basic MiniSearch options.
        <br><br><strong>🔍 SEE: </strong><em><a href="https://github.com/davemcdowell/OversizedSuit/tree/main#search-config-options">Search Config Options</a> or <a href="https://lucaong.github.io/minisearch/#md:search-options">MiniSearch Options</a> for more details on configuring MiniSearch.</em>
        <br><strong>📌 TODO: </strong><em>Allow ms config options or override object to pass directly to MiniSearch for full control over its options</em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <pre><code>
{
  msFields: ['title', 'description'],
  msStoreFields: ['title', 'type', 'description', 'date', 'url', ...], 
  msBoostFields: { 'title': 2 },
}</code></pre>
      </td>
    </tr>
    <tr>
      <td><code>filterLogic</code></td>
      <td>
        Custom method to define the filtering method that is used by faceted search.
        <br><br><strong>ℹ️ NOTE: </strong><em>If left null, the default <code>AND/OR</code> filtering will be used</em>.
      </td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <pre><code>
const mySearchLogic = {
  
}
        </code></pre>
      </td>
    </tr>
  </tbody>
</table>

#### Container Options
<table>
  <thead>
    <tr>
      <th>Option Name</th>
      <th>Summary</th>
      <th>Required?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>searchContainer</code></td>
      <td>Query selector that points to the parent search element which should contain the form that holds inputs for the faceted search.</td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Elements</strong></td>
      <td colspan="2"><code>data&#8209;search&#8209;input</code>, <code>data&#8209;clear&#8209;input</code>, <code>data&#8209;search&#8209;filter</code>, <code>data&#8209;clear&#8209;group&#8209;filters</code>, <code>data&#8209;clear&#8209;all&#8209;filters</code></td>
    </tr>
    <tr>
      <td><code>resultsContainer</code></td>
      <td>Query selector that points to the parent element that will contain results when rendered.</td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Elements</strong></td>
      <td colspan="2"><em>None</em></td>
    </tr>
  </tbody>
</table>

#### Search Config Options
<table>
  <thead>
    <tr>
      <th>Option Name</th>
      <th>Summary</th>
      <th>Required?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>msFields</code></td>
      <td>
        A string array holding field names to be used for MiniSearch text matching.
        <br><br><strong>ℹ️ NOTE: </strong><em>strings must match JSON key names exactly.</em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>['title', 'description']</code></td>
    </tr>
    <tr>
      <td><code>msStoreFields</code></td>
      <td>
        A string array holding field names which should be returned in the result object(s) from MiniSearch.
        <br><br><strong>ℹ️ NOTE: </strong><em>strings must match JSON key names exactly. These key-values must be returned if they are to be used in faceted search!</em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>['title', 'type', 'description', 'price', 'url', 'locations', 'amenities', 'tags', 'vibes']</code></td>
    </tr>
    <tr>
      <td><code>msBoostFields</code></td>
      <td>An object containing key-value pairs for fields that should be boosted in search results along with the multiplier.</td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>{ 'title': 2 }</code></td>
    </tr>
  </tbody>
</table>

#### Pagination Options
<table>
  <thead>
    <tr>
      <th>Option Name</th>
      <th>Summary</th>
      <th>Required?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>paginationControls</code></td>
      <td>Query selector that points to the parent element which should contain all pagination control elements.</td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Elements</strong></td>
      <td colspan="2"><code>data&#8209;page&#8209;previous</code>, <code>data&#8209;page&#8209;next</code></td>
    </tr>
    <tr>
      <td><code>paginationProgressBar</code></td>
      <td>Query selector that points to the parent element containing a progress bar to show pagination progress. <em>Optional, but useful for visually showing user progress through result pages</em>.</td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Elements</strong></td>
      <td colspan="2"><code>data&#8209;page&#8209;progress</code></td>
    </tr>
    <tr>
      <td><code>resultsPerPage</code></td>
      <td>A number (positive integer) indicating how many results to show per page render, uses a fallback of 5 if not defined.</td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">10</td>
    </tr>  
  </tbody>
</table>

#### Summary Options
<table>
  <thead>
    <tr>
      <th>Option Name</th>
      <th>Summary</th>
      <th>Required?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>pageIndexSummary</code></td>
      <td>
        Query selector that points to the element to show pagination index summary info as text content. Typically used on a <code>output</code> element.
        <br><br><strong>ℹ️ NOTE: </strong><em>Outputs as 'Page X of Y'</em>
        <br><strong>📌 TODO: </strong><em>Allow custom output formats or options. i.e. Light, Verbose, etc...</em>
      </td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>&lt;output id="index-summary"&gt;...&lt;/output&gt;</code></td>
    </tr>
    <tr>
      <td><code>pageResultSummary</code></td>
      <td>
        Query selector that points to the element to show result(s) summary info as text content. Typically used on a <code>output</code> element.
        <br><br><strong>ℹ️ NOTE: </strong><em>Outputs as 'Showing (X - Y) of Z Results'</em>
        <br><strong>📌 TODO: </strong><em>Allow custom output formats or options. i.e. Light, Verbose, etc...</em>
      </td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Elements</strong></td>
      <td colspan="2"><code>&lt;output id="results-summary"&gt;...&lt;/output&gt;</code></td>
    </tr>
    <tr>
      <td><code>noResultsFeedback</code></td>
      <td>
        Query selector that points to a parent element that holds your custom no results feedback. Typically located within the results container.
        <br><br><strong>📌 TODO: </strong><em>Add data attributes to mark elements to show current user search queries, active facets and reset search button.</em>
      </td>
      <td>✖ No</td>
    </tr>
    <tr>
      <td><strong>Elements</strong></td>
      <td colspan="2"><em>None</em></td>
    </tr>  
  </tbody>
</table>

#### Range Options
> [!NOTE]
> Ranges are not required for OsS to work, this sections 'Required?' column assumes a range is being defined in the `ranges` object array option.

<table>
  <thead>
    <tr>
      <th>Option Name</th>
      <th>Summary</th>
      <th>Required?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>rangeInput</code></td>
      <td>Query selector that points to the range to be defined by this range object. Typically used on a <code>input</code> element of type <code>range</code>, but can also work with custom range sliders.</td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <code>&lt;input id="review-score" type="range" min="1" max="5"&gt;</code>
        <br><code>&lt;range-slider id="price-range" min="1" max="10000"&gt;&lt;/range-slider&gt;</code>
      </td>
    </tr>
    <tr>
      <td><code>rangeEvent</code></td>
      <td>
        Event to listen for when modifying this range's field(s). Typically 'change' event is best suited for this option.
        <br><br><strong>📌 TODO: </strong><em>Make debounce timing available as a range option to support 'input' events and prevent event flooding.</em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <code>'range-slider:change'</code>
        <br><code>'change'</code>
      </td>
    </tr>
    <tr>
      <td><code>rangeFields</code></td>
      <td>An object with key-value pairs to define fields that this range will make use of in its logic and URL state.</td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2"><code>{ fromPrice: 0, toPrice: 0 }</code></td>
    </tr>  
    <tr>
      <td><code>rangeInitiateMethod</code></td>
      <td>
        A custom method to initiate this range slider on load.
        <br><br><strong>ℹ️ NOTE: </strong><em>Method must include the parameters <code>(rangeElement, rangeFields)</code></em>
        <br><strong>📌 TODO: </strong><em>Auto-populate range fields based on URL state parameters; if any exist that match this range's <code>rangeFields</code>.</em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <pre><code>
//initiating a custom range slider with two fields
const initiatePriceRange = (rangeElement, rangeFields) => {
    const rangeValues = rangeElement.getNumberInputRangeValues();
    rangeFields.fromPrice = rangeValues.min;
    rangeFields.toPrice = rangeValues.max;
}</code></pre>
      </td>
    </tr>  
    <tr>
      <td><code>rangeEventMethod</code></td>
      <td>
        A custom method to define what happens when the event defined in <code>rangeEvent</code> is triggered.
        <br><br><strong>ℹ️ NOTE: </strong><em>Method must include the parameters <code>(event, rangeFields)</code></em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <pre><code>
//handle what happens when this range's event triggers
const priceChangeEvent = (event, rangeFields) => {
    rangeFields.fromPrice = event.detail.min;
    rangeFields.toPrice = event.detail.max;
}</code></pre>
      </td>
    </tr>
    <tr>
      <td><code>rangeFilterLogic</code></td>
      <td>
        A custom method to define how this range filters results during the result matching phase.
        <br><br><strong>ℹ️ NOTE: </strong><em>Method must include the parameters <code>(result, rangeFields)</code></em>
        <ul>
          <li><strong>result:</strong> <em>An object returned from search with expected key-values for use in fine-tuning search results.</em></li>
          <li><strong>rangeFields:</strong> <em>...</em></li>
        </ul>
        <br><strong>ℹ️ NOTE: </strong><em>Method must return true or false on a per-result basis.</code></em>
      </td>
      <td>✔ Yes</td>
    </tr>
    <tr>
      <td><strong>Example</strong></td>
      <td colspan="2">
        <pre><code>
//handle filtering results (per-result) for matching criteria
const priceFilter = (result, rangeFields) => {
    const resultPrice = result.price;
    if (rangeFields.fromPrice && resultPrice < rangeFields.fromPrice)
        return false;
    if (rangeFields.toPrice && resultPrice > rangeFields.toPrice)
        return false;
    return true;
}</code></pre>
      </td>
    </tr>  
  </tbody>
</table>

# 🖼️  Showcase
