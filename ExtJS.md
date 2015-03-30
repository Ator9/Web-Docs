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
          { text: 'Google Search', scope: this, handler: this.addAdwords, service: 'SEARCH', serviceID: 1 },
          { text: 'Google Display', scope: this, handler: this.addAdwords, service: 'DISPLAY', serviceID: 2 }
      ]
  })
}
```
