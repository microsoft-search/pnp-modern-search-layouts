
# Nation Flags in the refiner

One of the properties in the search result is the Managed Metadata field Nationality which is used as a refiner:

![TermSet Nationality](assets/termsetnationality.png)



However the customer prefers to see the flag of the country AND the name of the country in the refiner

![FlagsInRefiner](assets\flagsinrefiner.png)





The flags are provided by the [Flag CDN](https://flagcdn.com/) and all we have to do is to test the name of the country and covert it to the URL of the flag.
like this:

```handlebars
  {{#eq name 'USA'}}
    <pnp-img class="nationflag" src="https://flagcdn.com/32x24/us.png" hideOnError/>
{{/eq}} 

```

## Layout source code

<content id="data-content">
    
    <style>

		.filter {
			margin-bottom: 10px;
		}
    
		.filter--value {
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			display: flex;
			align-items: center;
		}

		.filter--values-list {
			overflow: auto;
			max-height: 400px;
		}

		.filter--message {
			padding: 10px;
		}
		
	    .filter--name {
			font-family: {{@root.theme.fonts.large.fontFamily}};
			font-weight: {{@root.theme.fonts.large.fontWeight}};
			font-size: {{@root.theme.fonts.large.fontSize}};
			margin-bottom: 5px;
		}

		.filter--option {
			margin-bottom: 8px;
			padding-left: 10px;
		}

		pnp-filtercheckbox {
			max-width: 100%;
		}
        .nationflag{
            position: absolute;
            right: 50px;
        }
				        		                
    </style>

    <div class="template vertical">  

		{{#each @root.filters as |filter|}}
			<div class="filter">
                {{#eq filter.displayName 'Nationality'}}
                {{#eq filter.selectedTemplate 'ComboBoxFilterTemplate'}}
                <div class="filter--name">{{filter.displayName}}</div>
                <div class="filter--value">
                    <pnp-filtercombobox 
                        data-instance-id="{{@root.instanceId}}" 
                        data-filter-name="{{filter.filterName}}" 
                        data-is-multi="{{filter.isMulti}}"
                        data-show-count="{{filter.showCount}}"
                        data-operator="{{filter.operator}}"
                        data-theme-variant="{{JSONstringify @root.theme}}"
                        >
                            {{#each filter.values}}
                                <option value="{{value}}" data-selected="{{selected}}" data-disabled="{{disabled}}" data-count="{{count}}">{{name}}</option>
                            {{/each}}
                    </pnp-filtercombobox>
                </div>
        {{else}}
            <pnp-collapsible data-group-name="{{filter.displayName}}" data-default-collapsed="{{#or hasSelectedValues expandByDefault selectedOnce}}false{{else}}true{{/or}}" data-theme-variant="{{JSONstringify @root.theme}}">
                
                <template id="collapsible-header">
                                                
                </template>
                
                <template id="collapsible-content">
                    {{#eq filter.selectedTemplate 'DateRangeFilterTemplate'}}
                        <div class="filter--value">
                            <pnp-filterdaterange 
                                data-instance-id="{{@root.instanceId}}" 
                                data-filter="{{JSONstringify filter 2}}"
                                data-theme-variant="{{JSONstringify @root.theme}}"
                            ></pnp-filterdaterange>
                        </div>
                    {{else}}
                        {{#eq filter.selectedTemplate 'CheckboxFilterTemplate'}}

                            {{#if filter.isMulti}}
                                <div class="filter--option">
                                    <pnp-filteroperator 
                                        data-instance-id="{{@root.instanceId}}"
                                        data-filter-name="{{filter.filterName}}" 
                                        data-operator="{{filter.operator}}" 
                                        data-theme-variant="{{JSONstringify @root.theme}}"
                                    ></pnp-filteroperator>
                                </div>
                            {{/if}}
                        
                            <div class="filter--option">
                                <pnp-filtersearchbox data-filter="{{JSONstringify filter}}" data-instance-id="{{@root.instanceId}}" data-theme-variant="{{JSONstringify @root.theme}}"></pnp-filtersearchbox>
                            </div>

                            <div class="filter--values-list">
                                {{#each filter.values}}
                                <div title="{{name}} {{#if ../showCount}}({{count}}){{/if}}" class="filter--value">
                            

                                    
                                    <pnp-filtercheckbox 
                                    data-instance-id="{{@root.instanceId}}" 
                                    data-filter-name="{{filter.filterName}}" 
                                    data-name={{name}} 
                                    data-value="{{value}}"
                                    data-selected="{{selected}}" 
                                    data-disabled="{{disabled}}" 
                                    data-count="{{count}}"
                                    data-is-multi="{{../isMulti}}"
                                    data-theme-variant="{{JSONstringify @root.theme}}"
                                >
                                </pnp-filtercheckbox>
                                {{#eq name 'Denmark'}} 
                                <pnp-img class="nationflag" src="https://flagcdn.com/32x24/dk.png" hideOnError/>
                                {{/eq}}

                                {{#eq name 'USA'}}
                                <pnp-img class="nationflag" src="https://flagcdn.com/32x24/us.png" hideOnError/>
                                {{/eq}} 
                                
                                {{#eq name 'UK'}}
                                <pnp-img class="nationflag" src="https://flagcdn.com/32x24/gb.png" hideOnError/>
                                {{/eq}} 
                                {{#eq name 'India'}}
                                <pnp-img class="nationflag" src="https://flagcdn.com/32x24/in.png" hideOnError/>
                                {{/eq}} 

                                        {{#if ../showCount}}
                                            <span data-ui-test-id="filterCount">({{count}})</span>
                                        {{/if}}
                                    </div>
                                {{/each}}
                            </div>
                        {{else}}
                            {{#eq filter.selectedTemplate 'DateIntervalFilterTemplate'}}
                                <div class="filter--value">
                                    <pnp-filterdateinterval 
                                        data-instance-id="{{@root.instanceId}}" 
                                        data-filter="{{JSONstringify filter 2}}"
                                        data-theme-variant="{{JSONstringify @root.theme}}"
                                    >
                                </pnp-filterdateinterval>
                                </div>
                            {{/eq}}	
                        {{/eq}}
                    {{/eq}}

                </template>

                <template id="collapsible-footer">
                    {{#isnt filter.selectedTemplate 'DateRangeFilterTemplate'}}
                            {{#eq filter.values.length 0}}
                                <div class="filter--message">
                                    {{@root.strings.FilterNoValuesMessage}}
                                </div>
                            {{else}}
                                {{#if filter.isMulti}}
                                    <pnp-filtermultiselect 
                                        data-instance-id="{{@root.instanceId}}" 
                                        data-filter-name="{{filter.filterName}}" 
                                        data-apply-disabled="{{#if filter.canApply}}false{{else}}true{{/if}}" 
                                        data-clear-disabled="{{#if filter.canClear}}false{{else}}true{{/if}}">
                                    </pnp-filtermultiselect>
                                {{/if}}
                            {{/eq}}
                    {{/isnt}}
                </template>

            </pnp-collapsible>
        {{/eq}}
                {{else}}
                    {{#eq filter.selectedTemplate 'ComboBoxFilterTemplate'}}
                            <div class="filter--name">{{filter.displayName}}</div>
                            <div class="filter--value">
                                <pnp-filtercombobox 
                                    data-instance-id="{{@root.instanceId}}" 
                                    data-filter-name="{{filter.filterName}}" 
                                    data-is-multi="{{filter.isMulti}}"
                                    data-show-count="{{filter.showCount}}"
                                    data-operator="{{filter.operator}}"
                                    data-theme-variant="{{JSONstringify @root.theme}}"
                                    >
                                        {{#each filter.values}}
                                            <option value="{{value}}" data-selected="{{selected}}" data-disabled="{{disabled}}" data-count="{{count}}">{{name}}</option>
                                        {{/each}}
                                </pnp-filtercombobox>
                            </div>
                    {{else}}
                        <pnp-collapsible data-group-name="{{filter.displayName}}" data-default-collapsed="{{#or hasSelectedValues expandByDefault selectedOnce}}false{{else}}true{{/or}}" data-theme-variant="{{JSONstringify @root.theme}}">
                            
                            <template id="collapsible-header">
                                                            
                            </template>
                            
                            <template id="collapsible-content">
                                {{#eq filter.selectedTemplate 'DateRangeFilterTemplate'}}
                                    <div class="filter--value">
                                        <pnp-filterdaterange 
                                            data-instance-id="{{@root.instanceId}}" 
                                            data-filter="{{JSONstringify filter 2}}"
                                            data-theme-variant="{{JSONstringify @root.theme}}"
                                        ></pnp-filterdaterange>
                                    </div>
                                {{else}}
                                    {{#eq filter.selectedTemplate 'CheckboxFilterTemplate'}}

                                        {{#if filter.isMulti}}
                                            <div class="filter--option">
                                                <pnp-filteroperator 
                                                    data-instance-id="{{@root.instanceId}}"
                                                    data-filter-name="{{filter.filterName}}" 
                                                    data-operator="{{filter.operator}}" 
                                                    data-theme-variant="{{JSONstringify @root.theme}}"
                                                ></pnp-filteroperator>
                                            </div>
                                        {{/if}}
                                    
                                        <div class="filter--option">
                                            <pnp-filtersearchbox data-filter="{{JSONstringify filter}}" data-instance-id="{{@root.instanceId}}" data-theme-variant="{{JSONstringify @root.theme}}"></pnp-filtersearchbox>
                                        </div>

                                        <div class="filter--values-list">
                                            {{#each filter.values}}
                                                <div title="{{name}} {{#if ../showCount}}({{count}}){{/if}}" class="filter--value">
                                                    <pnp-filtercheckbox 
                                                        data-instance-id="{{@root.instanceId}}" 
                                                        data-filter-name="{{filter.filterName}}" 
                                                        data-name="{{name}}" 
                                                        data-value="{{value}}" 
                                                        data-selected="{{selected}}" 
                                                        data-disabled="{{disabled}}" 
                                                        data-count="{{count}}"
                                                        data-is-multi="{{../isMulti}}"
                                                        data-theme-variant="{{JSONstringify @root.theme}}"
                                                    >
                                                    </pnp-filtercheckbox>
                                                    {{#if ../showCount}}
                                                        <span data-ui-test-id="filterCount">({{count}})</span>
                                                    {{/if}}
                                                </div>
                                            {{/each}}
                                        </div>
                                    {{else}}
                                        {{#eq filter.selectedTemplate 'DateIntervalFilterTemplate'}}
                                            <div class="filter--value">
                                                <pnp-filterdateinterval 
                                                    data-instance-id="{{@root.instanceId}}" 
                                                    data-filter="{{JSONstringify filter 2}}"
                                                    data-theme-variant="{{JSONstringify @root.theme}}"
                                                >
                                            </pnp-filterdateinterval>
                                            </div>
                                        {{/eq}}	
                                    {{/eq}}
                                {{/eq}}

                            </template>

                            <template id="collapsible-footer">
                                {{#isnt filter.selectedTemplate 'DateRangeFilterTemplate'}}
                                        {{#eq filter.values.length 0}}
                                            <div class="filter--message">
                                                {{@root.strings.FilterNoValuesMessage}}
                                            </div>
                                        {{else}}
                                            {{#if filter.isMulti}}
                                                <pnp-filtermultiselect 
                                                    data-instance-id="{{@root.instanceId}}" 
                                                    data-filter-name="{{filter.filterName}}" 
                                                    data-apply-disabled="{{#if filter.canApply}}false{{else}}true{{/if}}" 
                                                    data-clear-disabled="{{#if filter.canClear}}false{{else}}true{{/if}}">
                                                </pnp-filtermultiselect>
                                            {{/if}}
                                        {{/eq}}
                                {{/isnt}}
                            </template>

                        </pnp-collapsible>
                    {{/eq}}
                {{/eq}}
			</div>		
    	{{/each}}
	</div>
</content>


