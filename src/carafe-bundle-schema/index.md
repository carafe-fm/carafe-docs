# Introduction

A Carafe Bundle is a JSON document that encapsulates everything needed to develop, share, and deploy JavaScript in a FileMaker WebViewer.

A Carafe Bundle has a formal schema which can be validated using [JSON Schema](https://json-schema.org/), a vocabulary that allows us to annotate and validate JSON documents.

The formal schema allows all the Carafe tooling (and any other tooling that understand JsonSchema such as many code editors) to validate Bundles.

Over time as the format of Bundles changes, having a schema definition will allow code editors and other tooling to properly handle different schemas of Bundles over time without confusion and/or backward compatibility breaks.

# Details

The current JsonSchema Bundle schema canonical id is `https://carafe.fm/schema/draft-01/bundle.schema.json`

In plain English, the core properties of a Bundle are as follows:

* `html`
    * A template which contains _merge fields_ which match the top level property names defined in `config`.
    * Typically, in addition to HTML and _merge fields_, this template will contain JavaScript, CSS.
    * When merged properly, the resulting valid HTML document provides some display or interactivity as designed.
    
* `config`
    * An Object with properties describing rules for each of the _merge fields_ in `html`.
        * `isData`
            * `false`
                * _SHOULD NOT_ be included in the `data` Object.
                * _MUST_ be merged at compile time.
                * _MUST NOT_ be merged by a deployed FileMaker script at runtime.
            * `true`
                * _MUST_ be included in the `data` Object with sample data.
                * _MUST_ be merged by a FileMaker script at run time.
        * `value`
            * _MUST NOT_ be empty when `isData` is `false`
        * `help`
            * _MAY_ be included to clarify the purpose of the property.
        * `category`
            * _MAY_ be included to group the property in tooling which recognizes it.
        * `label`
            * _MAY_ be included to label the property in tooling which recognizes it.
        * `sort`
            * _MAY_ be included to sort the property in tooling which recognizes it.
        * `type`
            * _MAY_ be use of the following values for compatibility with WidgetStudio, but not enforced by the validator. See Config Example below for usage examples.
                * `booleanCheck` `json array` `json object` `number` `text`
                * `CSS Snippet` `HTML Snippet` `JS Snippet`
                * `color` `padding` `textAlign` `verticalAlign`
                * `fmpFileName` `fmpScriptName`
                * `dropdown` `popup`
        * `possibleValues`
            * _MAY_ be included with `dropdown` or `popup` properties in tooling which recognizes it.
        * Additional miscellaneous keys
            * _MAY_ be included to support tooling which recognize them.
                
    * Config sub-schema canonical id is `https://carafe.fm/schema/draft-01/config.schema.json`

* `data`
    * A Object with properties containing sample data for each of the _merge fields_ which are specified as requiring example data, and which _MUST BE_ merged by FileMaker at run time.
    * Data sub-schema canonical id is `https://carafe.fm/schema/draft-01/data.schema.json`

* `meta` 
    * Several additional properties which define details about how to merge and deploy the three core properties, as well as information about creator, version, description, etc.
        * `bookend`
            * A string used as a delimiter for the _merge fields_. The default bookend is a pair of underscore characters, but can be defined as needed so as not to collide with code in the html template.
        * `about`
            * _REQUIRED_ explanation about how to implement. May contain simple HTML markup.
        * `description`
            * _REQUIRED_ text description of the purpose of the bundle.
        * `id`
            * _REQUIRED_ GUID/UUID
        * `creator` `category` `name` `version`
            * _REQUIRED_ string attribution and identification properties
        *  `hashLoad` `offlineCompatibile` `windowsTested`
            * _REQUIRED_ loosely typed Boolean properties as defined in `https://carafe.fm/schema/draft-01/bundle.schema.json#/definitions/boolean`
        * `bridgeAPIMethods`
            * _REQUIRED_ JSON Object containing zero or more bridge method definitions. See schema and/or example below for more details.
        * `references`
            * _REQUIRED_ JSON Array containing zero or more URIs to developer references.
                      
        * Any additional meta properties added to a Bundle are valid but will ignored by Carafe
    * Meta sub-schema canonical id is `https://carafe.fm/schema/draft-01/meta.schema.json`

* `preview` `previewName`
    * Contains a Base64 encoded jpg image and the filename for it.
   
    
# Config Example

This is a complex example showing many examples of config properties as might be generated by other tooling and still be valid Carafe config schema draft-01.

```json
{
    "$schema" : "https://carafe.fm/schema/draft-01/config.schema.json#",
    "booleanVar": {
        "category": "General Inputs",
        "help": "Optional help text. Values should be string keywords \"true\" or \"false\"",
        "isData": false,
        "label": "Boolean",
        "sort": "1",
        "type": "booleanCheck",
        "value": "false"
    },
    "colorVar": {
        "category": "Style",
        "help": "Optional help text.",
        "isData": false,
        "label": "Color",
        "sort": "1",
        "type": "color",
        "value": "rgba(121,121,121,1)"
    },
    "cssSnippetVar": {
        "category": "Snippets",
        "help": "Optional help text.",
        "isData": false,
        "label": "CSS",
        "sort": "2",
        "type": "CSS Snippet",
        "value": ".selector:  { height: 100%; }"
    },
    "dropdownVar": {
        "category": "General Inputs",
        "help": "Optional help text. Enumerated list which expects other values.",
        "isData": false,
        "label": "Dropdown",
        "possibleValues": [
            "Baz",
            "Bat"
        ],
        "sort": "4",
        "type": "dropdown",
        "value": "Baz"
    },
    "fmpFileNameVar": {
        "calculation": "Get(FileName)",
        "category": "FileMaker",
        "help": "Optional help text.",
        "isData": false,
        "label": "FMP File Name",
        "sort": "1",
        "type": "fmpFileName",
        "value": "Carafe"
    },
    "fmpScriptNameVar": {
        "category": "FileMaker",
        "help": "Optional help text.",
        "isData": false,
        "label": "FMP Script Name",
        "lengthSafeParam": true,
        "parameterData": {
            "theKey": "This is the explainer of theKey"
        },
        "sort": "2",
        "type": "fmpScriptName",
        "value": "My Script"
    },
    "htmlSnippetVar": {
        "category": "Snippets",
        "help": "Optional help text.",
        "isData": false,
        "label": "HTML Snippet",
        "sort": "1",
        "type": "HTML Snippet",
        "value": "<h1>Some HTML</h1>"
    },
    "jsSnippetVar": {
        "category": "Snippets",
        "help": "Optional help text.",
        "isData": false,
        "label": "JS Snippet",
        "sort": "3",
        "type": "JS Snippet",
        "value": "alert('jsSnippetVar ran.');"
    },
    "jsonArrayVar": {
        "category": "JSON",
        "help": "Optional help text.",
        "isData": true,
        "label": "JSON Array",
        "sort": "1",
        "type": "json array"
    },
    "jsonObjectVar": {
        "category": "JSON",
        "help": "Optional help text.",
        "isData": true,
        "label": "JSON Object",
        "sort": "2",
        "type": "json object"
    },
    "numberVar": {
        "category": "General Inputs",
        "help": "Optional help text.",
        "isData": false,
        "label": "Number",
        "sort": "2",
        "type": "number",
        "value": 3.14
    },
    "paddingVar": {
        "category": "Style",
        "help": "Optional help text.",
        "isData": false,
        "label": "Padding",
        "sort": "2",
        "type": "padding",
        "value": "1px 2px 3px 4px"
    },
    "popupVar": {
        "category": "General Inputs",
        "help": "Optional help text. Enumerated list which expects only enumerated values.",
        "isData": false,
        "label": "Popup",
        "possibleValues": [
            "Foo",
            "Bar"
        ],
        "sort": "5",
        "type": "popup",
        "value": "Foo"
    },
    "textAlignVar": {
        "category": "Style",
        "help": "Optional help text.",
        "isData": false,
        "label": "Text Align",
        "sort": "3",
        "type": "textAlign",
        "value": "left"
    },
    "textVar": {
        "category": "General Inputs",
        "help": "Optional help text.",
        "isData": true,
        "label": "Text",
        "sort": "3",
        "type": "text"
    },
    "verticalAlignVar": {
        "category": "Style",
        "help": "Optional help text.",
        "isData": false,
        "label": "Vertical Align",
        "sort": "4",
        "type": "verticalAlign",
        "value": "top"
    }
}
```
# Meta bridgeAPIMethods Example

This is an example showing how the meta property might be generated by other tooling and still be valid Carafe meta schema draft-01.

```json
    // snip
    "bridgeAPIMethods": {
        "exampleBridgeMethodName": {
            "callback": false,
            "description": "Bridge method with no callback"
        },
        "exampleBridgeMethodWithCallbackName": {
            "callback": true,
            "description": "Bridge method with callback modified"
        }
    },
    // snip
```

# Compatibility

Carafe tooling does not require Bundles to declare a schema. It will duck-type any bundle that does not declare a schema and attempt to validate it anyway.

# Contributing

It is our intent by defining a schema for Bundles to facilitate stronger interoperability of tools. We encourage community dialog about what the schema definition should be. In draft-01 we have kept the constraints quite loose in an attempt to facilitate maximum interoperability.

Discussion and contributions encouraged in the [Carafe Bundler project on GitHub](https://github.com/soliantconsulting/carafe-bundler/tree/master/schema)
