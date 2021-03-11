# Accessing Opening Hours, Time zones and Public Holidays

The opening hours is a crucial part of place information.

When considering presenting opening hours to a user, you must observe several
user interface considerations:

- Places can have multiple opening hours for a day
- Places can be open always/24H or open till late
- Places may indicate if they are open on public holidays, but can often not be
  kept updated
- Places can provide comments against a status
- A user may wish to know the opening hours for a place in local time, such as
  if they need reach out during open time

## Opening Hours

Various place providers engage operators to contribute their opening hours.
These are made accessible through the API in order for you to present to the
user.

- Use `intervals()` to access the next series of opening hours for a place. Use
  this to present the immediate open/closed times
- Use `forDays()` to query a range of dates for opening hours for a place. Use
  this to present a weeks worth of opening hours, or possibly more (such as
  a calendar).

### Upcoming Open/Closed hours using `intervals()` connection

The `intervals()` operation provides you the information about the immediate
next open/closed hours for a place. You are provided back a series of
`from`/`to` window intervals along with the associated `status` and `comment`.

This operation was designed to assist user interfaces that wish to present the
next time a venue is open, or perform more nuanced interface messages, such as
"opening soon" or "closing soon".

```graphql
# Query the next series of open/closed hours for a place, in order to present
# information whether the venue is open, closed, opening soon, closing soon,
# etc.

query QueryUpcomingOpenClosedTimes {
  # Use the place() operation
  place(id: "place/facebook:page:mavisthegrocer") {
    # Access the opening hours (where specified)
    hours {
      # Access the next window that the place is open. Supports selecting a
      # series of Open/Closed hours and can be used to let users know when
      # a venue will be open/closed next.
      intervals(first: 1, status: Open) {
        nodes {
          # Times from/to
          from
          to
          # Status Open/Closed
          status
          # Any comments attached to the opening hours
          comment
        }
      }
    }
    # Access the time zone for this location, which can be used for presenting
    # localised dates.
    attr(id: "place/time-zone") {
      value
    }
  }
}
```

### Open/Closed hours for a specific date range

The `forDays()` operation allows you to define a date range, such as for the
next day, week or future calendar range. For each of the dates that are
selected, you will be provided the `intervals()` operations to access the
open/closed window intervals.

```graphql
# Obtain the open/closed hours for the place given a period, such as the
# upcoming week or future date range. Includes information on local public
# holidays that could affect opening hours.

query QueryDateRangeOpenClosedTimes {
  # Use the place() operation
  place(id: "place/facebook:page:mavisthegrocer") {
    # Access the opening hours (where specified)
    hours {
      # Select the period (e.g. the week between the from/to dates). Also access
      # the local public holidays in order to present to the user and let them
      # know of any events that could affect regular trading hours.
      forDays(from: "2021-04-01", to: "2021-04-07") {
        # Date
        date
        # Whether there is a public holiday for this date detected for this
        # region on this date
        publicHoliday
        # Obtain the intervals for this date, requesting the opening status
        # status is optional, otherwise use Open/Closed to specify your pref
        intervals(status: Open) {
          # Hours
          from
          to
          # Status (Open/Closed)
          status
          # Any corresponding comment for the opening hours
          comment
        }
      }
    }
    # Access the time zone for this location
    attr(id: "place/time-zone") {
      value
    }
  }
}
```

## Time zone

For places accessed on the Alpaca platform, we make available an attribute
to access the time zone. The time zone can enable you to locally adjust the
hours against the hours the place is open, or present the opening hours in
local time.

| Attribute ID      | Value                            | Example     |
| ----------------- | -------------------------------- | ----------- |
| `place/time-zone` | Time zone for the place position | "UTC+10:00" |