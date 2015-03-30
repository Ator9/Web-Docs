## Grid. Insert with default value
```js
Ext.create('Ext.ux.GridRowInsert', { defaultValues: { percent: 100 } })
```

## Menu
```js
{
  text: Admin.t('Add'),
  icon: 'resources/icons/plus.png',
  menu: new Ext.menu.Menu({
      plain: true,
      items: [
          { text: 'Google Search', serviceID: 1 },
          { text: 'Google Display', serviceID: 2 }
      ]
  })
}
```
