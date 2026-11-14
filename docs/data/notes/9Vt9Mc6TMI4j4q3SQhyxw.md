
## SSR Styled components in Next
Styled-components supports concurrent SSR (server side rendering) with stylesheet rehydration. The basic idea is that when your app renders on the server, you can create a ServerStyleSheet and add a provider to your React tree which accepts styles via a context API. This doesn’t interfere with global styles, such as keyframes or createGlobalStyle and allows you to use styled-components with React DOM’s various SSR APIs.
