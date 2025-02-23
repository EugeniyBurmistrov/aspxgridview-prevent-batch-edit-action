<!-- default badges list -->

![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/134059439/21.2.4%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/T496531)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)

<!-- default badges end -->

# Grid View for ASP.NET Web Forms - Prevent the cell edit action on the client in batch edit mode


<!-- run online -->

**[[Run Online]](https://codecentral.devexpress.com/134059439/)**

<!-- run online end -->

This example handles the client-side [`FocusedCellChanging`](https://docs.devexpress.com/AspNet/js-ASPxClientGridView.FocusedCellChanging) event to disable the cell edit action in [batch edit mode](https://docs.devexpress.com/AspNet/16443/components/grid-view/concepts/edit-data/batch-edit-mode) based on a condition defined in code.


> **Limitation:** This technique is not applicable if you set the [`EditMode`](https://docs.devexpress.com/AspNet/DevExpress.Web.GridViewBatchEditSettings.EditMode) property to **Row**. A user can focus and edit any cell in a row switched to edit mode except for cells that belong to read-only columns (the column's [`GridViewDataColumn.ReadOnly`](https://docs.devexpress.com/AspNet/DevExpress.Web.GridViewDataColumn.ReadOnly) property is set to `true`).

## Implementation Details

The following client-side [`FocusedCellChanging`](https://docs.devexpress.com/AspNet/js-ASPxClientGridView.FocusedCellChanging) event handler sets the [`e.Cancel`](https://docs.devexpress.com/AspNet/js-ASPxClientCancelEventArgs.cancel) property to `true` to cancel the focus action and subsequent edit operations for cells in specific columns and rows. The code uses the [`e.cellInfo`](https://docs.devexpress.com/AspNet/js-ASPxClientGridViewFocusedCellChangingEventArgs.cellInfo) event property to get information about the clicked cell.

```js
function onFocusedCellChanging(s, e) {
  if (e.cellInfo.column.name == "command") e.cancel = true;
  else if (e.cellInfo.column.fieldName == "SupplierID") e.cancel = true;
  else if (
    e.cellInfo.column.fieldName == "UnitsInStock" &&
    (e.cellInfo.rowVisibleIndex < 3 || e.cellInfo.rowVisibleIndex > 7)
  )
    e.cancel = true;
  else if (
    e.cellInfo.column.fieldName == "UnitPrice" &&
    s.batchEditApi.GetCellValue(e.cellInfo.rowVisibleIndex, "UnitPrice") > 22
  )
    e.cancel = true;
}
```

## Files to Look At

- [Default.aspx](./CS/Default.aspx) (VB: [Default.aspx](./VB/Default.aspx))
- [Default.aspx.cs](./CS/Default.aspx.cs) (VB: [Default.aspx.vb](./VB/Default.aspx.vb))

## Documentation

- [Batch Edit Mode](https://docs.devexpress.com/AspNet/16443/components/grid-view/concepts/edit-data/batch-edit-mode)

## More Examples

- [Grid View for ASP.NET Web Forms - A simple batch editing implementation](https://github.com/DevExpress-Examples/aspxgridview-simple-batch-editing-implementation)
- [Grid View for ASP.NET Web Forms - Editing an in-memory dataset](https://github.com/DevExpress-Examples/aspxgridview-edit-in-memory-dataset)
