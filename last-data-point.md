# Last Data Point Measure

## Description
This measure displays values only for the latest date within the current filter context, highlighting the most recent data point.

**Use case:** Display only the most current data in visuals while filtering out historical dates.

## DAX Code
```dax
Last data point =
VAR MaxDate =
    MAX ( DimCalendar[Date] )
VAR LatestDate =
    CALCULATE ( MAX ( DimCalendar[Date] ), ALLSELECTED ( DimCalendar[Date] ) )
VAR Result =
    IF ( MaxDate = LatestDate, SUM ( FactSales[Sales] ), BLANK () )
RETURN
    Result
```

## How it works
- `MaxDate`: Gets the maximum date in the current filter context
- `LatestDate`: Gets the maximum date ignoring visual-level filters (but respecting slicers)
- Returns values only when both dates match (i.e., it's the latest date)
