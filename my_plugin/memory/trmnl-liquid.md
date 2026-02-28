# TRMNL Liquid Templating Reference

---

## Basic Syntax

### Output
```liquid
{{ variable }}
{{ location.city }}
{{ items[0].name }}
```

### Assignment
```liquid
{% assign my_variable = "value" %}
{% assign temp = forecast.temperature | round %}
```

### Capture (multi-line assignment)
```liquid
{% capture svg_logo %}
<svg>...</svg>
{% endcapture %}
```

### Conditionals
```liquid
{% if condition %}
  ...
{% elsif other_condition %}
  ...
{% else %}
  ...
{% endif %}

{% unless condition %}...{% endunless %}

{% case variable %}
  {% when "value1" %} ...
  {% when "value2" %} ...
  {% else %} ...
{% endcase %}
```

### Loops
```liquid
{% for item in collection %}
  {{ item.name }}
  {{ forloop.index }}      <!-- 1-based -->
  {{ forloop.index0 }}     <!-- 0-based -->
  {{ forloop.first }}      <!-- true/false -->
  {{ forloop.last }}       <!-- true/false -->
  {{ forloop.length }}     <!-- total items -->
{% endfor %}

{% for item in collection limit: 5 offset: 2 %}...{% endfor %}

{% for i in (1..5) %}...{% endfor %}
```

### Filter Chaining
```liquid
{{ value | divided_by: total | times: 100 | round }}
{{ name | upcase | truncate: 20 }}
```

---

## Standard Liquid Filters

### String
- `upcase`, `downcase` - Case conversion
- `capitalize` - Capitalize first letter
- `truncate: N` - Truncate to N characters
- `truncatewords: N` - Truncate to N words
- `strip` - Remove whitespace
- `lstrip`, `rstrip` - Left/right strip
- `strip_html` - Remove HTML tags
- `strip_newlines` - Remove newlines
- `newline_to_br` - Convert newlines to `<br>`
- `escape` - HTML escape
- `url_encode` - URL encode
- `base64_encode` - Base64 encode
- `prepend: "prefix"` - Prepend string
- `append: "suffix"` - Append string
- `replace: "old", "new"` - Replace first occurrence
- `replace_first: "old", "new"` - Replace first only
- `remove: "str"` - Remove all occurrences
- `remove_first: "str"` - Remove first only
- `slice: start, length` - Substring
- `split: ","` - Split into array

### Math
- `plus: N` - Addition
- `minus: N` - Subtraction
- `times: N` - Multiplication
- `divided_by: N` - Division
- `modulo: N` - Remainder
- `round` - Round to nearest integer
- `round: N` - Round to N decimal places
- `ceil` - Round up
- `floor` - Round down
- `abs` - Absolute value
- `at_least: N` - Min value
- `at_most: N` - Max value

### Array
- `first` - First element
- `last` - Last element
- `size` - Array length
- `sort` - Sort ascending
- `sort: "key"` - Sort by key
- `sort_natural` - Case-insensitive sort
- `reverse` - Reverse order
- `uniq` - Remove duplicates
- `compact` - Remove nil values
- `join: ", "` - Join to string
- `map: "key"` - Extract values by key
- `where: "key", "value"` - Filter by key-value
- `concat: other_array` - Merge arrays

### Date
- `date: "%Y-%m-%d"` - Format date (strftime)
- Common formats: `%B %d, %Y` (January 01, 2025), `%H:%M` (14:30)

### Misc
- `default: "fallback"` - Default if nil/empty/false
- `json` - Convert to JSON (also TRMNL custom)

---

## TRMNL Custom Filters

### Data
- `json` - Convert variable to JSON string
- `parse_json` - Parse nested JSON strings back to objects

### Collections
- `group_by: "key"` - Organize arrays by key
  ```liquid
  {% assign grouped = items | group_by: "category" %}
  {% for group in grouped %}
    {{ group.name }}: {{ group.items | size }}
  {% endfor %}
  ```
- `find_by: "key", "value"` - Locate item by key-value pair (with optional fallback)
  ```liquid
  {{ items | find_by: "id", 42 }}
  ```
- `where_exp: "item", "item.score > 90"` - Filter with custom expression
  ```liquid
  {% assign high_scores = items | where_exp: "item", "item.score > 90" %}
  ```
- `sample` - Randomly select from comma-separated values
  ```liquid
  {{ "apple,banana,cherry" | sample }}
  ```

### Numbers
- `number_with_delimiter` - Format with separators
  ```liquid
  {{ 1234567 | number_with_delimiter }}           --> 1,234,567
  {{ 1234567 | number_with_delimiter: ' ', ',' }} --> 1 234 567,0
  ```
- `number_to_currency` - Convert to currency with locale
  ```liquid
  {{ 1234.56 | number_to_currency }}
  ```

### Strings/Markup
- `pluralize: "singular", "plural"` - Singular/plural forms
  ```liquid
  {{ count }} {{ count | pluralize: "item", "items" }}
  ```
- `markdown_to_html` - Convert markdown to HTML
  ```liquid
  {{ "**bold** text" | markdown_to_html }}
  ```

### Dates
- `days_ago: N` - Calculate date N days ago
  ```liquid
  {{ "now" | days_ago: 7 }}  <!-- 7 days ago -->
  ```
- `ordinalize` - Ordinal suffixes with strftime
  ```liquid
  {{ "2025-01-01" | ordinalize: "%B %-d" }}  --> January 1st
  ```

### Localization
- `l_date: format, locale` - Locale-specific date formatting
  ```liquid
  {{ "2025-01-15" | l_date: "%B %d", "fr" }}  --> janvier 15
  ```
- `l_word: locale` - Translate common words
  ```liquid
  {{ "Monday" | l_word: "fr" }}  --> lundi
  ```

### Other
- `append_random` - Generate unique ID for HTML elements
  ```liquid
  <div id="chart-{{ "container" | append_random }}">
  ```
- `qr_code` - Convert string/URL to scannable QR code image
  ```liquid
  {{ "https://example.com" | qr_code }}
  ```

---

## Global Variables

### User
- `{{ trmnl.user.name }}` - Full name
- `{{ trmnl.user.first_name }}` - First name
- `{{ trmnl.user.last_name }}` - Last name
- `{{ trmnl.user.locale }}` - Locale (e.g. "en")
- `{{ trmnl.user.utc_offset }}` - UTC offset in seconds (e.g. -14400)
- `{{ trmnl.user.time_zone }}` - Friendly name (e.g. "Eastern Time (US & Canada)")
- `{{ trmnl.user.time_zone_iana }}` - IANA ID (e.g. "America/New_York")

### Device
- `{{ trmnl.device.friendly_id }}` - Device friendly ID
- `{{ trmnl.device.percent_charged }}` - Battery percentage
- `{{ trmnl.device.wifi_strength }}` - WiFi signal
- `{{ trmnl.device.height }}` - Screen height (e.g. 480)
- `{{ trmnl.device.width }}` - Screen width (e.g. 800)

### System
- `{{ trmnl.system.timestamp_utc }}` - Current UTC timestamp

### Plugin
- `{{ trmnl.plugin_settings.instance_name }}` - Plugin instance name

---

## Template Tag (Shared Markup)

### Define Template
```liquid
{% template say_hello %}
Hello there, {{ name }}.
{% endtemplate %}
```

### Render Template
```liquid
{% render "say_hello", name: "General Kenobi" %}
```

### Shared Markup File (`shared.liquid`)
- Content is prepended to every view layout before rendering
- Use for common styles, scripts, and reusable templates
- Define `{% template %}` blocks here for use across all views

```liquid
<!-- shared.liquid -->
<style>
.custom-class { font-weight: bold; }
</style>

{% template weather_icon %}
<img class="image-dither" src="https://api.jodet.com/weather_icon/{{ code }}" />
{% endtemplate %}
```

---

## Timezone Handling
- All timestamps from TRMNL are UTC
- Use `trmnl.user.utc_offset` for manual offset
- Use `trmnl.user.time_zone_iana` for IANA timezone
- In local dev (`.trmnlp.yml`): set `time_zone: America/New_York`

---

## Merge Variable Syntax
- TRMNL web editor: `##{{ variable }}` (double hash prefix)
- trmnlp local dev: `{{ variable }}` (standard Liquid)
- Multiple polling URLs: `{{ IDX_0.field }}`, `{{ IDX_1.field }}`

---

## Inline SVG Pattern
```liquid
{%- capture svg_logo %}
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
  <path d="M12 0L24 12L12 24L0 12Z"/>
</svg>
{%- endcapture %}

<img src="data:image/svg+xml;base64,{{ svg_logo | base64_encode }}">
```

---

## Liquid-JavaScript Integration
Use Liquid to inject data into JavaScript for charts:
```liquid
<script>
var chartData = {{ data | json }};
// Use chartData with Highcharts/Chartkick
</script>
```
