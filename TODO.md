
## TODOs

- high level plan
  - better theme mechanics
  - fork own default theme
  - get widgety

- let _config do it's job, theme declared
  - some other theme related vars will be around

- location of theme files doesn't change much.
  - how they are loaded will

- aiming for a dependency chain theme setup... at minimum:
  - In this order of preference for render:
    - local
    - theme
      - parent theme
    - default theme
  - doesn't look like we can file test, so we'll go declaratively
    - we'll probably have a configured list of the theme chain
    - config space will have a list for each theme (via _data if we need) of what theme includes it provides
    - a functional include file that runs a name against a test through the namespaces of the theme include lists, falling back through the chain.



