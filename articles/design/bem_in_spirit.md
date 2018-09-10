In a recent environment, I found [BEM -- block element modifier based CSS](https://en.bem.info/articles/learning-to-love-bem/) to be the rule of design and more often than not, found it very counter-natural to not use the cascading nature of tags to structure documents. 

In my own practice I am finding "Quasi BEM" to be more practical then pure BEM methodology. 

The rule of BEM is that 

1. Every element has a class
2. That class is either a block tag or an element. elements are suffixed children of blocks as in `'login-form'` .. `'login-form__submit-button'`
3. cascading/element styles are not allowed
4. All modifiers are localized to a block or element

I went through a hate-love-mixed reaction to BEM and now use a slightly modified version. 

## There are no absolute good or bad in structured design

BEM regards colliding CSS definitions as the worst evil possible, and is a system designed to block that worst evil completely. And like most absolute philosophies, it kind of expects the universe to warp itself into a confirming shape. 

It also claims that the elements are not in and of themselves meaningful contributions to the visual namespace. 

This is again, a very contextual thing.