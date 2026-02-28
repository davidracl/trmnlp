# TRMNL Template Examples & Patterns

---

## Basic Full-Screen Template
```html
<div class="view view--full">
  <div class="layout layout--col gap">
    <span class="title">Hello World</span>
    <span class="description">A simple TRMNL plugin</span>
  </div>
  <div class="title_bar">
    <img class="image" src="/images/plugins/trmnl--render.svg">
    <span class="title">My Plugin</span>
    <span class="instance">v1.0</span>
  </div>
</div>
```

---

## Weather Dashboard (Current my_plugin)
Grid layout with indoor/outdoor temps, weather icons, precipitation, and 2-day forecast.
```html
<div class="layout">
  <div class="grid grid--cols-2 gap--large">
    <!-- Indoor Temperature -->
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        <span class="value value--large value--tnums"
          >{{ netatmo[0].dashboard_data.Temperature | number_with_delimiter: ' ', ','}}°</span>
        <span class="label label--medium">T° intérieure</span>
      </div>
    </div>
    <!-- Outdoor Temperature -->
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        <span class="value value--large value--tnums"
          >{{ netatmo[0].modules[0].dashboard_data.Temperature | number_with_delimiter: ' ', ','}}°</span>
        <span class="label label--medium">T° extérieure</span>
      </div>
    </div>
    <!-- Weather Icon + Condition -->
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        {% assign weatherCode = forecast.timelines.daily[0].values.weatherCodeMax | append: "" %}
        <span class="value value--medium">
          <img class="image-dither" src="https://api.jodet.com/weather_icon/{{ weatherCode }}" />
        </span>
        <span class="label label--medium">{{ weatherCodes.fr[weatherCode] }}</span>
      </div>
    </div>
    <!-- Precipitation -->
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        <span class="value value--medium"
          >{{ forecast.timelines.daily[0].values.precipitationProbabilityAvg | number_with_delimiter: ' ', ',' }}%</span>
        <span class="label label--medium">Chance de pluie</span>
      </div>
    </div>
    <!-- Min/Max Temps -->
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        <span class="value value--large value--tnums"
          >{{ forecast.timelines.daily[0].values.temperatureMin | number_with_delimiter: ' ', ','}}°</span>
        <span class="label label--medium">T° minimale</span>
      </div>
    </div>
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        <span class="value value--large value--tnums"
          >{{ forecast.timelines.daily[0].values.temperatureMax | number_with_delimiter: ' ', ','}}°</span>
        <span class="label label--medium">T° maximale</span>
      </div>
    </div>
    <!-- 2-Day Forecast -->
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        {% assign weatherCode = forecast.timelines.daily[1].values.weatherCodeMax | append: "" %}
        <span class="label label--small">Demain</span>
        <span class="value value--medium">
          <img class="image-dither" src="https://api.jodet.com/weather_icon/{{ weatherCode }}" />
        </span>
        <span class="label label--medium">
          {{ forecast.timelines.daily[1].values.temperatureMin | round | number_with_delimiter: ' ', ','}}° |
          {{ forecast.timelines.daily[1].values.temperatureMax | round | number_with_delimiter: ' ', ','}}° |
          {{ forecast.timelines.daily[1].values.precipitationProbabilityAvg | round | number_with_delimiter: ' ', ','}}%
        </span>
      </div>
    </div>
    <div class="item">
      <div class="meta"></div>
      <div class="content">
        {% assign weatherCode = forecast.timelines.daily[1].values.weatherCodeMax | append: "" %}
        <span class="label label--small">Après-demain</span>
        <span class="value value--medium">
          <img class="image-dither" src="https://api.jodet.com/weather_icon/{{ weatherCode }}" />
        </span>
        <span class="label label--medium">
          {{ forecast.timelines.daily[2].values.temperatureMin | round | number_with_delimiter: ' ', ','}}° |
          {{ forecast.timelines.daily[2].values.temperatureMax | round | number_with_delimiter: ' ', ','}}° |
          {{ forecast.timelines.daily[2].values.precipitationProbabilityAvg | round | number_with_delimiter: ' ', ','}}%
        </span>
      </div>
    </div>
  </div>
</div>
```

---

## Rich Text / Article Pattern
```html
<div class="layout">
  <div class="columns">
    <div class="column">
      <div class="richtext richtext--center gap--large">
        <span class="title" data-pixel-perfect="true">Article Title</span>
        <div class="content" data-content-limiter="true">
          <p>Article body text goes here. The content limiter will
          automatically resize text to fit the available space.</p>
        </div>
      </div>
    </div>
  </div>
</div>
<div class="title_bar">
  <img class="image" src="icon.svg">
  <span class="title">Articles</span>
  <span class="instance">Daily Digest</span>
</div>
```

---

## Todo List / Item List with Overflow
```html
<div class="layout layout--col layout--stretch gap">
  <div class="columns" data-overflow="true" data-overflow-counter="true">
    <div class="column">
      <span class="label label--small" data-group-header="true">High Priority</span>
      <div class="item">
        <div class="meta"><span class="index">1</span></div>
        <div class="content">
          <span class="title title--small">Fix login bug</span>
          <span class="description" data-clamp="1">Users can't login with SSO</span>
        </div>
      </div>
      <div class="item">
        <div class="meta"><span class="index">2</span></div>
        <div class="content">
          <span class="title title--small">Update dependencies</span>
          <span class="description" data-clamp="1">Security patches needed</span>
        </div>
      </div>
      <span class="label label--small" data-group-header="true">Low Priority</span>
      <div class="item">
        <div class="meta"><span class="index">3</span></div>
        <div class="content">
          <span class="title title--small">Refactor auth module</span>
        </div>
      </div>
    </div>
  </div>
</div>
<div class="title_bar">
  <img class="image" src="icon.svg">
  <span class="title">Tasks</span>
</div>
```

---

## Data Dashboard Pattern (Grid + Values)
```html
<div class="layout layout--col gap">
  <div class="grid grid--cols-2 gap--medium">
    <div class="flex flex--col gap--xsmall">
      <span class="label">Revenue</span>
      <span class="value value--xlarge lg:value--xxlarge value--tnums"
        data-value-format="true" data-value-locale="en-US">${{ revenue }}</span>
    </div>
    <div class="flex flex--col gap--xsmall">
      <span class="label">Orders</span>
      <span class="value value--xlarge lg:value--xxlarge value--tnums"
        data-value-format="true">{{ orders }}</span>
    </div>
    <div class="flex flex--col gap--xsmall">
      <span class="label">Conversion</span>
      <span class="value value--xlarge value--tnums">{{ conversion }}%</span>
    </div>
    <div class="flex flex--col gap--xsmall">
      <span class="label">Avg. Order</span>
      <span class="value value--xlarge value--tnums"
        data-value-format="true">${{ avg_order }}</span>
    </div>
  </div>
</div>
```

---

## Table Pattern
```html
<div class="layout layout--col gap">
  <table class="table table--small" data-table-limit="true">
    <thead>
      <tr>
        <th><span class="title">Name</span></th>
        <th><span class="title">Status</span></th>
        <th><span class="title">Value</span></th>
      </tr>
    </thead>
    <tbody>
      {% for item in items %}
      <tr>
        <td><span class="label" data-clamp="1">{{ item.name }}</span></td>
        <td><span class="label label--outline">{{ item.status }}</span></td>
        <td><span class="value value--xxsmall value--tnums">{{ item.value }}</span></td>
      </tr>
      {% endfor %}
    </tbody>
  </table>
</div>
```

---

## Progress Bars Pattern
```html
<div class="layout layout--col gap">
  {% for goal in goals %}
  <div class="progress-bar">
    <div class="content">
      <span class="label">{{ goal.name }}</span>
      <span class="value value--xxsmall">{{ goal.percent }}%</span>
    </div>
    <div class="track">
      <div class="fill" style="width: {{ goal.percent }}%"></div>
    </div>
  </div>
  {% endfor %}
</div>
```

---

## Mashup Layout: 1 Left + 1 Right
```html
<div class="mashup mashup--1Lx1R">
  <div class="view view--half_vertical">
    <div class="layout layout--col gap">
      <span class="value value--xlarge">{{ left_value }}</span>
      <span class="label">Left Panel</span>
    </div>
    <div class="title_bar">
      <img class="image" src="icon1.svg">
      <span class="title">Left</span>
    </div>
  </div>
  <div class="view view--half_vertical">
    <div class="layout layout--col gap">
      <span class="value value--xlarge">{{ right_value }}</span>
      <span class="label">Right Panel</span>
    </div>
    <div class="title_bar">
      <img class="image" src="icon2.svg">
      <span class="title">Right</span>
    </div>
  </div>
</div>
```

---

## Mashup Layout: 2x2 Grid
```html
<div class="mashup mashup--2x2">
  <div class="view view--quadrant">
    <div class="layout layout--center">
      <span class="value value--large">Q1</span>
    </div>
    <div class="title_bar">
      <img class="image" src="icon.svg">
      <span class="title">Quadrant 1</span>
    </div>
  </div>
  <div class="view view--quadrant">
    <div class="layout layout--center">
      <span class="value value--large">Q2</span>
    </div>
    <div class="title_bar">
      <img class="image" src="icon.svg">
      <span class="title">Quadrant 2</span>
    </div>
  </div>
  <div class="view view--quadrant">
    <div class="layout layout--center">
      <span class="value value--large">Q3</span>
    </div>
    <div class="title_bar">
      <img class="image" src="icon.svg">
      <span class="title">Quadrant 3</span>
    </div>
  </div>
  <div class="view view--quadrant">
    <div class="layout layout--center">
      <span class="value value--large">Q4</span>
    </div>
    <div class="title_bar">
      <img class="image" src="icon.svg">
      <span class="title">Quadrant 4</span>
    </div>
  </div>
</div>
```

---

## Responsive Design Pattern
```html
<div class="layout layout--col gap">
  <!-- Stack on small, side-by-side on medium+ -->
  <div class="flex flex--col md:flex--row gap--medium">
    <div class="flex flex--col gap--small stretch">
      <span class="label">Metric A</span>
      <span class="value value--large md:value--xlarge lg:value--xxlarge">{{ metric_a }}</span>
    </div>
    <div class="flex flex--col gap--small stretch">
      <span class="label">Metric B</span>
      <span class="value value--large md:value--xlarge lg:value--xxlarge">{{ metric_b }}</span>
    </div>
  </div>

  <!-- Hide on 1-bit, show on 2-bit+ -->
  <div class="hidden 2bit:block 4bit:block">
    <span class="description">Additional detail only visible on grayscale displays</span>
  </div>

  <!-- Different label styles per bit-depth -->
  <span class="label 1bit:label--inverted 2bit:label--outline 4bit:label--underline">Adaptive</span>
</div>
```

---

## Conditional Rendering / Error State
```liquid
{% if items and items.size > 0 %}
  <div class="layout layout--col gap">
    {% for item in items %}
      <div class="item">
        <div class="content">
          <span class="title title--small">{{ item.name }}</span>
        </div>
      </div>
    {% endfor %}
  </div>
{% else %}
  <div class="layout layout--center">
    <div class="flex flex--col flex--center gap">
      <span class="value">No Data</span>
      <span class="description">Configure this plugin with your API key</span>
    </div>
  </div>
{% endif %}
```

---

## Chart Pattern (Highcharts)
```html
<div class="layout layout--col gap">
  <div id="chart-container" class="stretch-x" style="height: 300px;"></div>
</div>

<script>
function createChart() {
  new Chartkick.LineChart("chart-container", {{ chart_data | json }}, {
    colors: ["#000000"],
    library: {
      chart: { backgroundColor: "transparent" },
      plotOptions: {
        series: {
          animation: false,
          enableMouseTracking: false,
          states: { hover: { enabled: false } },
          marker: { enabled: false }
        }
      },
      tooltip: { enabled: false },
      legend: { enabled: false },
      credits: { enabled: false },
      yAxis: { gridLineDashStyle: "shortdot" },
      xAxis: { gridLineDashStyle: "dot" }
    }
  });
}

if ("Chartkick" in window) {
  createChart();
} else {
  window.addEventListener("chartkick:load", createChart, true);
}
</script>
```

---

## Inline SVG Pattern
```liquid
{%- capture svg_icon %}
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="black">
  <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2z"/>
</svg>
{%- endcapture %}

<img src="data:image/svg+xml;base64,{{ svg_icon | base64_encode }}" class="w--8 h--8">
```

---

## QR Code Pattern
```html
<div class="layout layout--center">
  <div class="flex flex--col flex--center gap--medium">
    <span class="title">Scan to Visit</span>
    {{ "https://example.com" | qr_code }}
    <span class="description">example.com</span>
  </div>
</div>
```

---

## Item with Icon Pattern
```html
<div class="item">
  <div class="icon">
    <img src="data:image/svg+xml;base64,{{ icon_svg | base64_encode }}"
         class="w--[6cqw] h--[6cqw] portrait:w--[10cqw] portrait:h--[10cqw]" />
  </div>
  <div class="content">
    <span class="title title--small">{{ item.name }}</span>
    <span class="description" data-clamp="2">{{ item.description }}</span>
    <div class="flex gap--small">
      <span class="label label--small label--outline">{{ item.tag }}</span>
      <span class="label label--small">{{ item.date }}</span>
    </div>
  </div>
</div>
```
