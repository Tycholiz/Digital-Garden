
Formik keeps track of your form’s state and then exposes it plus a few reusable methods and event handlers (handleChange​, handleBlur​, and handleSubmit​) to your form via props​. 
- `handleChange​` and `handleBlur​` work exactly as expected — they use a name​ or id​ attribute to figure out which field to update.
- Formik handles validation and errors for us as well

The button with `type="submit"` is what submits the Formik form. 
- like HTML `<form>`, the default behaviour is to send a GET and put the form values in the URL.

### `withFormik`

#### `handleSubmit`
helps with the form submission in Formik. It is automatically passed the form values and any other props wrapped in the component

#### `mapPropsToValues`
used to initialize the values of the form state. Formik transfers the results of mapPropsToValues​ into an updatable form state and makes these values available to the new component as props.values.
- helps to transform the outer props into form values. It returns all the values gotten from the form details.

### Components
#### `<Field />`
used to automatically set up React forms with Formik. 
- It’s able to get the value by using the `name` attribute, which it uses to match up the Formik state 
- it is by default set to the `input` element. That can easily be changed by specifying a component prop.

We can use the `<Field />` component to get values in and out of Formik internal state

```tsx
// text input
{touched.fullname && errors.fullname && <p>{errors.fullname}</p>}
<Field className="input" type="text" name="fullname" placeholder="Full Name" />

// select
{touched.editor && errors.editor && <p>{errors.editor}</p>}
<div className="control">
  <Field component="select" name="editor">
    <option value="atom">Atom</option>
    <option value="sublime">Sublime Text</option>
  </Field>
</div>

// checkbox
<label className="checkbox">
  {touched.newsletter && errors.newsletter && <p>{errors.newsletter}</p>}
  <Field type="checkbox" name="newsletter" checked={values.newsletter} />
  Join our newsletter?
</label>
```

#### `<Form/>`
helper component ​​that extends the native form​ element. ​​It automatically assigns the `onSubmit​` event handler to props.handleSubmit

The component is used to encompass the whole code needed for the form.