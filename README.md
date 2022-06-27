# School Search

This simple UI uses Vue 3 in Vite.

It accepts an input for College Name and displays a list of matching Colleges and puts markers on a map to show where the college results exist.

## Project Setup

```sh
npm install
```

## Environment Keys

This project uses API Keys for Google Platform Maps and College Scorecard

https://mapsplatform.google.com/

https://api.data.gov/signup/

Once you have those keys, copy the `.env` file to `.env.development.local` and replace the keys in the local file.

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
