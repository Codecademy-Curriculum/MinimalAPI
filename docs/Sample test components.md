This is a validation rule to ensure the learner correctly binds the Razor page to the PizzaModel class using the @model directive.

```
if (!Components.CodeEditor.codeContains('Pages/Pizza.cshtml', /\s*@model\s*PizzaModel\s*/)) {
  return {
    pass: false,
    errors: {
      friendly: 'Did you use the `@model` directive to specify the correct model?',
      component: 'PersistentCodeEditor'
    }
  };
}
return { pass: true };
```
