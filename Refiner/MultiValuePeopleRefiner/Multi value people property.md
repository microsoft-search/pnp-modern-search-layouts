
# Making Multi value people property refiner looks better

Triggered by this(https://github.com/microsoft-search/pnp-modern-search/discussions/3582) discussions in the repository, I have created a new layout that will make the multi value people property refiner looks better. 

First step is to map the OWS_Q_USER_[InternalFieldName] property to a RefinableStringXX property in the search schema. 

Then we need to add the RefinableStringXX property to the refiner.
When we add the RefinableStringXX property to the refiner we get this:

![Outofthebox](/Refiner/MultiValuePeopleRefiner/MultivalueProperty_outofthebox.png)




Not exactly what we want. We want to show the name of the person.

![AfterUpdate](/Refiner/MultiValuePeopleRefiner/MultivalueProperty_after.png)


We have to update the layout to parse the value(name) and only show the name of the person. 

```
data-name= "{{#with (split name '|')}}{{[1]}}{{/with}}" 
```
But we also have to ensure that this does not mess up any other refiner we might have.
One way (and the one chosen here) is to check if the filterName is RefinableStringXXX. If it is we know that this is a multi value people property and we can parse the value. 

```
{{#eq filter.filterName 'RefinableString114'}}
    <the wrapper>
    data-name= "{{#with (split name '|')}}{{[1]}}{{/with}}" 
{{else}}
    <the wrapper>
    data-name="{{name}}" 
{{/eq}}
```
The complete layout looks like this:
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
				        		                
    </style>

    <div class="template vertical">  

		{{#each @root.filters as |filter|}}
			<div class="filter">
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
											{{#eq filter.filterName 'RefinableString114'}}
												<pnp-filtercheckbox 
													data-instance-id="{{@root.instanceId}}" 
													data-filter-name="{{filter.filterName}}" 
													data-name= "{{#with (split name '|')}}{{[1]}}{{/with}}" 

													data-value="{{value}}" 
													data-selected="{{selected}}" 
													data-disabled="{{disabled}}" 
													data-count="{{count}}"
													data-is-multi="{{../isMulti}}"
													data-theme-variant="{{JSONstringify @root.theme}}"
												>
												</pnp-filtercheckbox>

											{{else}}
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
			</div>		
    	{{/each}}
	</div>
</content>
```