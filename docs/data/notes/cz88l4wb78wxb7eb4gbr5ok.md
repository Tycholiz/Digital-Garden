
### Downsides of HoCs
- If we use HoCs to enact business logic, then we need to change the structure of our component tree so that the HoC wraps the sub components that use the business logic
  - this is a case where [[hooks|react.lang.hooks]] are more appropriate, as we don't have to alter our component structure.