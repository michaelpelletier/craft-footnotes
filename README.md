# Footnotes plugin for Craft

A simple yet powerful footnotes plugin for Craft. The plugin is in **private beta** right now.

## Installation

To install the plugin, copy the footnotes/ folder into craft/plugins/. Then go to Settings → Plugins and click the "Install" button next to "Footnotes".

If you plan using Footnotes with Redactor editor, you also need to enable the plugin's Redactor features. Open up your Redactor config files stored in craft/config/redactor/ and add `'footnotes'` to the `plugins` setting.

```javascript
{
  buttons: ['html','formatting','bold','italic','unorderedlist','orderedlist','link','image','video'],
  plugins: ['fullscreen','footnotes']
}
```


## Usage

To add a footnote to your text you need to place a footnote marker in the correct position in the text. The actual footnote is defined separately from the text it is refering to.

This can be a "Footnotes" field in your entry type or – if you're using Matrix – a field within a "Footnotes" Matrix block type.

For really long articles it is also possible to have multiple Matrix blocks, enabling the user to write the footnotes underneath each article section, for example.


### Field setup

- Create a new field to define footnotes in. There is no dedicated footnotes field type for this. Use a rich text (Redactor) or plain text field for it (the plugin parses its content for footnotes, following a bespoke syntax).
- Add your new field to the entry types you want to add footnotes to.


### Syntax styles

Footnote definitions and footnote markers can be written down in different syntax styles. The prefered syntax style can be configured in the plugin settings.

#### Syntax for definitions

- Redactor syntax:  
  `1. My footnote definition</p>`
- Plain text syntax  
  `1. My footnote definition`
- Markdown syntax  
  `[^1]: My footnote definition`

#### Syntax for markers

- Redactor syntax  
  `<fn-marker>1</fn-marker>`
- Markdown syntax  
  `[^1]`


### How to add a footnote

#### Marker

Add a footnote marker in the correct position in the text, following the chosen syntax.

If you're using Redactor editor, use the "Footnote" button in the editor's toolbar.

The connection between a marker and the footnote's content in your definition list is made through a freely selectable, unique name.

When rendered to your template, the plugin will number the footnotes list and the markers in order of markers' appearance in the text.

#### Definitions

Define the footnote content in the field you prepared to hold them, following the chosen syntax.

Write each footnote in a new line, if you're using plain text or markdown syntax style for your definitions.

When using Redactor, write each of them in a new paragraph.


### Templates

It takes three steps to do footnotes in your templates.

1. Pass the footnote definitions to the plugin using the Twig variable `craft.footnotes.setFootnotes()`.

2. Apply the plugin's `footnotes` Twig filter to each text that should be parsed for footnote markers.

   If the plugin can match the marker with a footnote definition, it replaces the marker with a link to the footnote. Unmatched markers are removed.

3. Render the footnotes list to the template using the Twig variable `craft.footnotes.getFootnotes()`.

#### Example

```html
{% do craft.footnotes.setFootnotes(entry.footnotes) %}

<article>
    <h1>{{ entry.title }}</h1>

    {{ entry.articleBody|footnotes }}

    <aside class="footnotes">
        {{ craft.footnotes.getFootnotes() }}
    </aside>
</article>
```


## Twig filter and variables

### `footnotes(articleId, markerSyntax)`

Parses a string for footnotes markers and replaces them with links to the corresponding footnote if one is found, otherwise removes the marker.

#### Parameters

`articleId` (optional)
:   Link footnotes to a specific article, in case there are multiple articles on a single page.

`markerSyntax` (optional)
:   Override the syntax style defined in the plugin settings.

### `craft.footnotes.setFootnotes(articleId, definitionSyntax)`

Parses a string for footnote definitions.

#### Parameters

`articleId` (optional)
:   Link footnotes to a specific article, in case there are multiple articles on a single page.

`definitionSyntax` (optional)
:   Override the syntax style defined in the plugin settings.

### `craft.footnotes.getFootnotes(articleId)`

Renders the footnotes list to the template.

#### Parameters

`articleId` (optional)
:   Link footnotes to a specific article, in case there are multiple articles on a single page.


## Plugin settings

### Footnotes parser

The prefered syntax styles the parser is expecting (→ available syntax styles) and an option to remove unmatched markers.

### Markup for footnote markers and lists

The markup for how footnote markers and lists are rendered can be modified by using your own templates. Create templates from scratch or edit upon examples provided with the plugin.

Save the templates anywhere in your craft/templates/ folder and specify the path in the plugin settings.
