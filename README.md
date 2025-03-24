# Formulatte ☕

A modern, type-safe form management library for Svelte 5+.

[![npm version](https://badge.fury.io/js/formulatte.svg)](https://badge.fury.io/js/formulatte)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Features

- 🎯 Built specifically for Svelte 5's runes
- 💪 Full TypeScript support
- 🔄 Built-in Yup validation
- ⚡ Lightweight and performant
- 🎨 Zero styling opinions
- 📦 Simple API
- 🔍 Detailed validation states
- 🛠️ Flexible field handling

## Installation

```bash
npm install formulatte
# or
pnpm add formulatte
# or
bun add formulatte
```

## Quick Start

```svelte
<script lang="ts">
  import { Form } from 'formulatte';
  import * as yup from 'yup';

  const schema = yup.object({
    email: yup.string().email().required(),
    password: yup.string().min(6).required()
  });

  const initialValues = {
    email: '',
    password: ''
  };

  async function handleSubmit(values) {
    console.log('Form submitted:', values);
  }
</script>

<Form {initialValues} validationSchema={schema} onSubmit={handleSubmit}>
  {#snippet default(form)}
    <div>
      <input 
        bind:value={form.state.values.email}
        onblur={() => form.validate('email')}
      />
      {#if form.state.errors.email}
        <span class="error">{form.state.errors.email}</span>
      {/if}
    </div>

    <button type="submit" disabled={!form.state.isValid}>
      Submit
    </button>
  {/snippet}
</Form>
```

## API Reference

### Form Component

The main component that manages form state and validation.

```typescript
interface FormProps<T> {
  initialValues: T;
  validationSchema?: Schema;
  onSubmit: (values: T) => Promise<void> | void;
  validateOnBlur?: boolean;
  validateOnChange?: boolean;
}
```

### Form State

Available through the form snippet parameter:

```typescript
interface FormState<T> {
  values: T;
  errors: Partial<Record<keyof T, string>>;
  touched: Partial<Record<keyof T, boolean>>;
  isSubmitting: boolean;
  isValid: boolean;
  isDirty: boolean;
}
```

### Form Methods

- `validate(field?: string)`: Validate entire form or specific field
- `submit()`: Submit the form
- `reset()`: Reset form to initial values
- `setFieldValue(field, value)`: Set field value
- `setFieldTouched(field, touched)`: Mark field as touched

## Examples

### With Field Validation

```svelte
<Form {initialValues} validationSchema={schema}>
  {#snippet default(form)}
    <input
      bind:value={form.state.values.email}
      onblur={() => form.validate('email')}
      class:error={form.state.errors.email && form.state.touched.email}
    />
  {/snippet}
</Form>
```

### With Custom Validation

```svelte
<Form
  initialValues={initialValues}
  onSubmit={async (values) => {
    const isValid = await customValidation(values);
    if (isValid) {
      // proceed
    }
  }}
>
  {#snippet default(form)}
    <!-- form fields -->
  {/snippet}
</Form>
```

## Contributing

We welcome contributions! Please see our [contributing guidelines](CONTRIBUTING.md) for details.

1. Fork it
2. Create your feature branch (`git checkout -b feature/amazing`)
3. Commit your changes (`git commit -am 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing`)
5. Create a new Pull Request

## Development

```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Run tests
npm test

# Build library
npm run package
```

## License

MIT © [Nathanael Bonfim]

See [LICENSE](LICENSE) for more information.

## Acknowledgments

- Inspired by Formik's API
- Built with Svelte 5's runes
- Created using [`sv`](https://npmjs.com/package/sv)

---

Made with lots of ☕ 
