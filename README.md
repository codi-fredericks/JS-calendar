# JS-calendar
a full calendar system in vanilla js


## CDN Links  
JS link
```
https://cdn.jsdelivr.net/gh/codi-fredericks/JS-calendar@1.1/calendar-min.js
```
```html
<script src="https://cdn.jsdelivr.net/gh/codi-fredericks/JS-calendar@1.1/calendar-min.js"></script>
```

default CSS theme
```
https://cdn.jsdelivr.net/gh/codi-fredericks/JS-calendar@1.1/calendar-min.css
```
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/codi-fredericks/JS-calendar@1.1/calendar-min.css">
```

## Create Calendar
```js
const calendar = new Calendar();
```
you can pass a dictionary of values as a paramiter to define aspects of the calendar

| Option                | Description | Default Value |
|----------------------|-------------|--------------|
| `default_view`       | Default calendar view (`month`, `week`, `day`, `agenda`). | `"month"` |
| `default_mobile_view` | Default view for mobile devices. | `"week"` |
| `span_events`        | Should multi-day events span across days? | `true` |
| `mobile_width`       | Width threshold for mobile view. | `900` |
| `auto_color`        | Auto-generate colors if not specified? | `true` |
| `show_offset`        | Show user’s UTC offset? | `false` |
| `24_hour_time`       | Use 24-hour time format? | `false` |
| `prefix with time` | prefix events with the time? | `true` |

> [!WARNING]  
> the calendar uses the `crypto.subtle` API and this feature is available only in [secure contexts](https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts) (HTTPS), in some or all [supporting browsers](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/subtle#browser_compatibility).

> [!NOTE]  
> In this event the calendar will default to the colors defined in the css file


## Add event
```js
await calendar.add_event()
// also supports
calendar.add_event()
```

this function accepts up to 3 parameters

### 1. **Event Information (Required)**  
A dictionary containing event details:  

| Key                 | Description |
|---------------------|-------------|
| `id`               | Unique event ID. |
| `title`            | Event title. |
| `start`  | Event start time (UNIX timestamp or `Date()` object). |
| `end`    | Event end time (UNIX timestamp or `Date()` object). |
| `tags`            | Array of tags (see below). |
| `all_day`         | Whether the event lasts all day (`true/false`). |

#### 1.1 **Tags Format**  
Each tag contains:  

| Key    | Description |
|--------|-------------|
| `name` | Tag name. |
| `color` | (Optional) Hex color code. |


### 2. **Event Metadata (Optional but recommended)**  
Additional event details for display purposes:  

| Key              | Description |
|----------------|-------------|
| `description`  | Event description. |
| `fields`       | Array of custom fields. |

#### 2.1 **Field Format**  
Each field contains:  

| Key          | Description |
|-------------|-------------|
| `title`     | Field title. |
| `value`     | Field content. |
| `inline`    | Display field inline? (default: `false`). |
| `custom_html` | Custom HTML content (renders inside an `iframe` in sandbox mode). |

> [!NOTE]  
>  Custom HTML is sandboxed inside an iframe for security reasons. 

### 3. **Render**
 `render` should the calendar re-render after adding this event

> [!TIP]
> Set `render` to `false` when adding multiple events to improve performance.  


## Render Calendar

```js
calendar.render();
```

This will force the calendar to update and display any newly added events.  


---
---
# Example useage

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/codi-fredericks/JS-calendar@1.1/calendar-min.css">
</head>
<body>
    <div id="calendar"></div>
    <script src="https://cdn.jsdelivr.net/gh/codi-fredericks/JS-calendar@1.1/calendar-min.js"></script>
    <script>
        const calendar = new Calendar();

        document.getElementById("calendar").appendChild(calendar.container);
        calendar.add_event(
            {
                id: "abc123",
                title: "Meeting with John",
                start: new Date(),
                end: new Date(Date.now() + 2 * 60 * 60 * 1000),
                tags:[
                    {
                        name:'john'
                    }
                ]
            },
            {
                description:"Just a small catchup with john to talk about those numbers",
                fields: [
                    {
                        title: 'office number',
                        value: '#441',
                        inline: true
                    },
                    {
                        title: 'address',
                        custom_html: `
                        <iframe src="https://www.google.com/maps/embed?pb=!1m16!1m12!1m3!1d129.59669646330195!2d2.343023007818176!3d48.88618874460989!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!2m1!1ssacre%20coeur!5e1!3m2!1sen!2snz!4v1742633528793!5m2!1sen!2snz" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
                        `
                    }
                ]
            }
        );
    </script>
</body>
</html>
```
