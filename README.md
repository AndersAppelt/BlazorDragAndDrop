# BlazorDragAndDrop

Small Blazor components for smooth and performant drag-and-drop:
- `Draggable` makes any UI block draggable.
- `DropZone<T>` receives drops and gives you typed drop context.

## How to use

1. Track the dragged item and its source list/column in page state.
2. Wrap each draggable item in `Draggable`.
3. Wrap each destination in `DropZone<T>` and pass the destination through `Context`.
4. In `OnDrop`, move the item from source to destination and clear drag state.

```razor
<DropZone T="Column" Context="@column" OnDrop="@HandleDrop">
    @foreach (var item in _items[column])
    {
        <Draggable OnDragStart="@(() => StartDrag(column, item))" OnDragEnd="@EndDrag">
            <FluentCard>@item.Title</FluentCard>
        </Draggable>
    }
</DropZone>
```

```csharp
private Item? _draggedItem;
private Column? _sourceColumn;

private void StartDrag(Column column, Item item)
{
    _sourceColumn = column;
    _draggedItem = item;
}

private void HandleDrop(Column target)
{
    if (_draggedItem is null || _sourceColumn is null) return;
    if (_items[_sourceColumn.Value].Remove(_draggedItem))
        _items[target].Add(_draggedItem);
    _draggedItem = null;
    _sourceColumn = null;
}

private void EndDrag()
{
    _draggedItem = null;
    _sourceColumn = null;
}
```
