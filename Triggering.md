**ITEM**(_id_, title, stock)

**USER**(_id_, name)

**ORDER_ITEM**(_id_, quantity, item_id, user_id)

> Create a trigger to decrement the quantity of item from Item relation when the user orders and item.

```sql
create or replace TRIGGER UPDATE_ITEM_QUANTITY
    BEFORE INSERT
    ON ORDER_ITEM
    FOR EACH ROW
BEGIN
        update item set item.in_stock = item.in_stock - :new.quantity where item.id = :new.item_id;
end;
```
