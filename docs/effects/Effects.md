# Effect development
Hyperion provides a powerful framework for developing your own effects, along with options and a user interface for tuning them.

## Effect Files 
An effect has 3 different files.
|         File          |               language                |                                        Comment                                        |
| :-------------------- | :------------------------------------ | :------------------------------------------------------------------------------------ |
| neweffect.py          | [Python](https://www.python.org)      | The heart of the effect                                                               |
| neweffect.json        | [JSON](https://www.json.org)           | Contains options for the python file, which makes it configurable                     |
| neweffect.schema.json | [JSON Schema](https://json-schema.org) | Creates the options UI and is used to validate user input. [Read more](/effects/Ui.md) |
