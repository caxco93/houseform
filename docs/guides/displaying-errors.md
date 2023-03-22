# Displaying errors

Whenever one of your `Field`s validation logic is rejected, the rejection string will be stored as an error. To ensure a proper user experience, it is important to inform your users of these errors by displaying them appropriately.

Fortunately, HouseForm provides various options to display the error message, allowing you to choose the one that best fits your use case.

## Displaying all Form Errors

The most straight forward way to show all the `Form` errors is by using the `errors` property from the child function.

```jsx
import { Form, Field } from "houseform";

<Form onSubmit={() => {}}>
  {({ submit, errors }) => (
    <div>
      <Field name="username"></Field>
      <Field name="password"></Field>
      <ul>
        {errors.map((error) => (
          <li key={error}>{error}</li>
        ))}
      </ul>
      <button onClick={submit}>Submit</button>
    </div>
  )}
</Form>;
```

This would display all the errors corresponding to that `Form`.

## Displaying errors per Field

In case you want to show the errors for each specific `Field`, you can do it by accessing the `errors` property on the `Field` child function.

```jsx
<Form onSubmit={() => {}}>
  {({ submit }) => (
    <div>
      <Field name="username">
        {({ errors }) => {
          return (
            <div>
              <input value={value} placeholder={"Username"} />
              {errors.map((error) => (
                <p key={error}>{error}</p>
              ))}
            </div>
          );
        }}
      </Field>
      <button onClick={submit}>Submit</button>
    </div>
  )}
</Form>
```

In this case, we are referencing the `errors` that correspond only to that specific Field. You can do this for each field if you want your errors to be closer to the input element.

## Alternative way to display errors

In case you want to access errors for each field to display them somewhere else inside your form, you can use the `errorsMap` property on the `Form` child function.

```jsx
import { Form, Field } from "houseform";

<Form onSubmit={() => {}}>
  {({ submit, errorsMap }) => (
    <div>
      <Field name="username"></Field>
      <Field name="password"></Field>
      <button onClick={submit}>Submit</button>
      <div>
        <p>Username specific errors</p>
        {errorsMap["username"]?.map((error) => (
          <p>{error}</p>
        ))}
      </div>
    </div>
  )}
</Form>;
```

## Set an Error from Form Submit Function

There are many times where you need to run validation on a submit. While HouseForm supports per-field submission validation using `onSubmitValidate` property, sometimes there's just no alternative to running validation in the `onSubmit` function.

But, in order to validate a form properly inside of the `onSubmit` function, you need a way to set errors on fields. The way to do this is by using the `getFieldValue` method on the `onSubmit`'s second passed argument, like so:

```jsx
<Form
  onSubmit={(values, form) => {
    form.getFieldValue("fieldName")?.setErrors(["This is an error"]);
  }}
>
  // ...
</Form>
```

This will cause the respective `Field` to re-render with the appropriate errors.
