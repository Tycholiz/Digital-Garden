
# Performance
- adding `shouldComponentUpdate` will drastically help with optimization.
    Consider this as you are building the app rather than considering it afterwards.
    - Alternatively, if you're concerned about a component re-rendering too many times, try using React.PureComponent
- [library to check how many times a component re-rendered needlessly](https://github.com/maicki/why-did-you-update)
    - the library is archived, but apparently works perfectly. alternatively: [why-did-you-render](https://github.com/welldone-software/why-did-you-render)
- [Spying the queue (focusing on the data passing over the bridge)](https://callstack.com/blog/react-native-how-to-check-what-passes-through-your-bridge/?ref=hackernoon.com)
