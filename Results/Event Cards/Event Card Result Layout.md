# Search Results for SharePoint Events
This template uses the reusable [pnp-documentcard](https://microsoft-search.github.io/pnp-modern-search/extensibility/web_components_list/#pnp-documentcard) web component to render items from a standard SharePoint Events list.

![Event Card Search Results](assets/eventCardResults.png)

## Additional Properties (in `selected properties`)
The following additional properies are needed:
`BannerUrlOWSURLH`
`EventsRollUpCategory`
`EventsRollUpEndDate`
`EventsRollUpStarteDate`
`ListItemID`
`Location`
`ParticipantsPickerOWSUSER`
`RefinableStringXX` - mapped to the EventsRollUpCategory crawled property

#### Optional LayoutSlots
| Slot name | Slot field | Description |
|:----------|:-----------|:------------|
| DefaultImage | Image URL | Default image to use when no banner is set |

## Results tempalte (rendering template)
The custom tempalte is built on HTML and CSS and uses the [pnp-documentcard](https://microsoft-search.github.io/pnp-modern-search/extensibility/web_components_list/#pnp-documentcard) web component. No additional plugins or libraries are needed.

```html
<content id="data-content">
    <style>
        /* Assign the correct size % ratio according to the number of wanted cards on a single row. We use flexbox instead of static grid to deal with SharePoint canvas sections*/
        .template--card {
            flex-grow: 1;
            flex-shrink: 1;
            min-width: 206px; /* Min width of the Office UI Fabric document card */
            flex-basis: 33%; /* fix percentaage */
        }   
        
        .template--location {
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            color:{{@root.theme.palette.bodyText}};
            font-style: normal;
            font-weight: normal;
            
        }
    </style>

    <div class="template">

        {{#if @root.properties.showSelectedFilters}}
            <pnp-selectedfilters 
                data-filters="{{JSONstringify filters.selectedFilters 2}}" 
                data-filters-configuration="{{JSONstringify filters.filtersConfiguration 2}}" 
                data-instance-id="{{filters.instanceId}}" 
                data-operator="{{filters.filterOperator}}"
                data-theme-variant="{{JSONstringify @root.theme}}"
            >
            </pnp-selectedfilters>
        {{/if}}
       
        <div class="template--header">
            {{#if @root.properties.showResultsCount}}
                <div class="template--resultCount">
                    <label class="ms-fontWeight-semibold">{{getCountMessage @root.data.totalItemsCount @root.inputQueryText}}</label>
                </div>
            {{/if}}

            <div class="template--sort">
                <pnp-sortfield 
                    data-fields="{{JSONstringify @root.properties.dataSourceProperties.sortList}}" 
                    data-default-selected-field="{{sort.selectedSortFieldName}}" 
                    data-default-direction="{{sort.selectedSortDirection}}"
                    data-theme-variant="{{JSONstringify @root.theme}}">
                </pnp-sortfield>    
            </div>
        </div>

        <div class="template--cardContainer">
            {{#each data.items as |item|}}    
            
                {{#> resultTypes item=item}}
                    <pnp-documentcard class="template--card" 
                        data-ui-test-id="resultCard" 
                        data-item="{{JSONstringify item}}" 
                        data-location="&lt;a style=&quot;color:{{@root.theme.palette.themePrimary}};font-weight:600;font-family:'{{@root.theme.fonts.small.fontFamily}}'&quot; href=&quot;{{@root.context.site.serverRequestPath}}?f=[{%22filterName%22:%22RefinableString08%22,%22values%22:[{%22name%22:%22{{EventsRollUpCategory}}%22,%22value%22:%22{{slot item @root.slots.Category}}%22,%22operator%22:0}]}]&quot;&gt;{{slot item @root.slots.Category}}&lt;/a&gt;&lt;br&gt;"
                        data-tags="{{#if (lt (moment (getDate EventsRollUpEndDate) diff=(getDate EventsRollUpStartDate)) '86400000')}}{{getDate EventsRollUpStartDate 'D, MMM yyyy'}}{{else}}{{#if (eq (getDate EventsRollUpStartDate 'MM')(getDate EventsRollUpEndDate 'MM'))}}{{getDate EventsRollUpStartDate 'D'}}-{{else}}{{getDate EventsRollUpStartDate 'D, MMM'}}&nbsp;-&nbsp;{{/if}}{{moment (moment EventsRollUpEndDate utc=null) format='D, MMM yyyy'}}{{/if}}&lt;/br&gt;{{#if Location}}&lt;span class=&quot;template--location&quot;&gt;{{Location}}&lt;/span&gt;{{else}}&nbsp;{{/if}}"
                        data-title="{{slot item @root.slots.Title}}"
                        data-preview-image="{{#if @root.slots.PreviewImageUrl}}{{slot item @root.slots.PreviewImageUrl}}{{elsse}}/sites/externalevents/SiteAssets/EventBanner.png{{/if}}" 
                        data-preview-url="{{SPSiteURL}}/_layouts/15/Event.aspx?ListGuid={{slot item @root.slots.ListId}}&ItemId={{slot item @root.slots.ID}}" 
                        data-date="{{#if (gt (length (split RefinableString120 ';')) 1)}}&nbsp;+{{minus (length (split RefinableString120 ';')) 1}}{{/if}}" 
                        data-href="{{SPSiteURL}}/_layouts/15/Event.aspx?ListGuid={{slot item @root.slots.ListId}}&ItemId={{slot item @root.slots.ID}}" 
                        data-author="{{#if RefinableString120}}{{RefinableString120}}{{else}}&nbsp;{{/if}}" 
                        data-profile-image="{{#if RefinableString120}}/_layouts/15/userphoto.aspx?size=L&username={{getUserEmail RefinableString120}}{{else}}data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII={{/if}}" 
                        data-file-extension="docx"
                        data-enable-preview="true" 
                        data-is-container="false"
                        data-show-file-icon="false"
                        data-is-compact="false" 
                        data-context="{{JSONstringify (truncateContext @root)}}"
                        data-instance-id="{{@root.instanceId}}"
                        data-theme-variant="{{JSONstringify @root.theme}}"
                        data-allow-item-selection="{{@root.properties.itemSelectionProps.allowItemSelection}}"
                        data-item-key="{{@root.paging.currentPageNumber}}{{@index}}"                     
                        data-selected-keys="{{JSONstringify @root.selectedKeys}}"
                        data-index="{{@index}}"
                        >
                    </pnp-documentcard>
                {{/resultTypes}}

            {{/each}}

            <!-- Generate fake items to correctly align cards on rows using flexbox -->
            {{#times 3}}
                <pnp-documentcard 
                    class="template--card" 
                    data-ui-test-id="fakeCard"
                    data-theme-variant="{{JSONstringify @root.theme}}"
                    style="visibility:hidden;height: 0;padding: 0">
                </pnp-documentcard>
            {{/times}}
        </div>

        {{#if @root.properties.paging.showPaging}}

            {{#gt @root.data.totalItemsCount @root.properties.paging.itemsCountPerPage}}
                <pnp-pagination 
                    data-total-items="{{@root.data.totalItemsCount}}" 
                    data-hide-first-last-pages="{{@root.properties.paging.hideFirstLastPages}}"
                    data-hide-disabled="{{@root.properties.paging.hideDisabled}}"
                    data-hide-navigation="{{@root.properties.paging.hideNavigation}}"
                    data-range="{{@root.properties.paging.pagingRange}}" 
                    data-items-count-per-page="{{@root.properties.paging.itemsCountPerPage}}" 
                    data-current-page-number="{{@root.paging.currentPageNumber}}"
                    data-theme-variant="{{JSONstringify @root.theme}}"
                >
                </pnp-pagination>
            {{/gt}}
            
        {{/if}}

    </div>

</content>

<content id="placeholder-content">   

        <style>
            /* Assign the correct size % ratio according to the number of wanted cards on a single row. We use flexbox instead of static grid to deal with SharePoint canvas sections*/
            .template--card {
                flex-grow: 1;
                flex-shrink: 1;
                min-width: 206px; /* Min width of the Office UI Fabric document card */
                flex-basis: 33%;
            }    
        </style>

    <div class="placeholder">

        {{!-- #if showResultsCount --}}
        <div class="template--resultCount">
            <span class="placeholder--shimmer placeholder--line" style="width: 20%"></span>
        </div>
        {{!-- /if --}}
        <div class="template--cardContainer">

            {{#times @root.properties.paging.itemsCountPerPage}}
                <pnp-documentcardshimmers class="template--card" 
                    data-ui-test-id="fakeCard"
                    data-theme-variant="{{JSONstringify @root.theme}}"
                    data-is-compact="false">
                </pnp-documentcardshimmers>
            {{/times}}

            <!-- Generate fake items to correctly align cards on rows using flexbox -->
            {{#times 3}}
                <pnp-documentcard class="template--card" 
                    data-ui-test-id="fakeCard"
                    data-theme-variant="{{JSONstringify @root.theme}}"
                    style="visibility:hidden;height: 0;padding: 0">
                </pnp-documentcard>
            {{/times}}
        </div>

    </div>
</content>
```
## Query template suggestion
List all Events on the current site (make sure to select the properties listed above).

```{searchTerms} Path:{Site} contentclass:"STS_ListItem_Events"```

👉 If you want to filter the Events to a specific category, add refinement filter: `FEventsRollUpCategory: "Category value"`

