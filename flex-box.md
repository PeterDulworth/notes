# Flex Box

To use flex-box:

```css
crossbow {
    display: flex;
}
```

Flexbox defaults to `flex-direction: row`. This is the direction that the children will be placed (left to right).

```css
crosbow {
    display: flex;
    flex-direction: row || column || row-reverse || column-reverse;
}
```

Flexbox defaults to `justify-content: flex-start`. This lays out all the elements justified towards the beginning of the flex box. 

```css
crossbow {
    display: flex;
    flex-direction: row; /* default */
    justify-content: flex-start || flex-end || center || space-between || space-around;
}
```

