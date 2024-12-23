# Passing Data To Vue Components Externally

## Main file

```php

<stafiz-select
  id="types"
  name="types"
  @change="() => {$('#expenseCustomFields').trigger('filterCustomFieldsTrigger');window.stafizSelectTypes();}"
  :items="[
    @foreach ($exptypes as $exptype)
      {{ json_encode([
        'value' => (string)$exptype->id,
        'label' => ($exptype->system != "french_ik" ? $exptype['type'] : __('settings.TXT_SETTINGS_FRENCHIKNAM')) .
          (($exptype->system == "french_ik" && isset($appli_country_ik)) ? ' (' . $appli_country_ik . ')' : '')
        ]) }},
    @endforeach
  ]"
  required
  default-first>
</stafiz-select>

<additional-fields-form
  :custom_fields="{{json_encode($customFields)}}"
  field_categories="types"
  id_form="savovo"
  id="expenseCustomFields"
>
</additional-fields-form>
```

This is our file. We have two vue components in a blade file and we're passing the ID 
from the `stafiz-select` component to the `additional-fields-form` component. To do this we add a jquery function 
on the `stafiz-select` `@change` vue flag where we pass the same ID as `id="expenseCustomFields"` in the `additional-fields-form` component.

The `trigger('filterCustomFieldsTrigger')` signals the `additional-fields-form` component to update.

Now we need to add inside `additional-fields-form`component 
```html
`<div ref="filterCustomFieldsRef" :id="id"></div>
```

this `ref` vue flag is the name of the trigger we passed on the jquery called in `@change`, `filterCustomFieldsRef`.

We also need to add a prop

```js
        id:{
            required: false,
            default: "expenseCustomFields"
        }
```

This will bind our id in id="expenseCustomFields" on the `additional-fields-form` component.

Additionally, we also add a method 

```vue
    methods: {
        filterCustomFields()
        {
            this.value = $('#types').val();
        },
```

This method retrieves the selected `value` from the `stafiz-select` component (which has the ID types) 
and assigns it to the `value` `data` property of the `additional-fields-form component`.

```vue
mounted() {
        $(this.$refs.filterCustomFieldsRef).on('filterCustomFieldsTrigger', this.filterCustomFields);
    }
```

The mounted hook binds the `filterCustomFieldsTrigger` event to the `filterCustomFields` method.
The `filterCustomFields` method fetches the new value from the `stafiz-select` component and
updates the `value` data property in `additional-fields-form`.

This sets up an event listener for the `filterCustomFieldsTrigger` event.
When the event is triggered (via jQuery in the `@change` handler of stafiz-select),
it will call the `filterCustomFields` method in the `additional-fields-form` component.
