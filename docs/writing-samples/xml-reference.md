---
title: XML Reference
---

# Volume Range Types

???+ example "Meta"

    * **Tool**: Readme.io in Markdown
    * **Company**: DirectScale
    * **Published**: [https://developers.directscale.com/docs/rangetype](https://developers.directscale.com/docs/rangetype)

This reference details the various Range Types used in a Commission Plan Template. 

At the beginning of a Commission Plan, you must always have a `<ComPeriod>` with the template's details. 
Within that, you have a single `<VolumeRange>` that serves as the Commission Plan's default date range. 

Use Volume Ranges to set separate date ranges to count volume. Each Volume Range has a `Name` attribute, set as anything you want. 

You can reference the name throughout the Commission Plan. The core part of the Volume Ranges is the `<RangeType>`.

Range Types are a period for which you are calculating commissions. The most common Range Types are `<Monthly>`,  `<Weekly>`, and  `<BiMonthly>`.

## `<Monthly>`

A Range starting on the first of the month and ending on the last. Use the `Offset` parameters to include multiple months. Typically, you only use this for a sub-Volume Range, not your main.

| Attribute      | Description                          |
| ----------- | ------------------------------------ |
| `StartOffset`       | Takes the main Volume Range and offsets the start date to the past by the amount entered. For example, if the current month is January and you declare `StartOffset=1`, then the `<Monthly>` range starts on December 1st.  |
| `EndOffset`       | Takes the main Volume Range and offsets the start date to the future by the amount entered. For example, if the current month is January and you declare `EndOffset=1`, then the `<Monthly>` range ends on February 28th. |

### Example

The following example looks at the current and previous months.

=== "XML"

    ```xml
    <VolumeRange Name="FirstFullMonth">
        <RangeType>
            <Monthly StartOffset="1" EndOffset="0" />
        </RangeType>
    </VolumeRange>
    ```

## `<Weekly>`

A range of a week (7 days), beginning on the day you define as the `WeekBegin`.

| Attribute | Description |
|---------------|--------------|
| `WeekBegin`  | Assign the day of the week the range begins on (`Monday`, `Tuesday`, `Wednesday`, and so on).

### Example

With the following example, the Volume Range starts on `Monday`.

=== "XML"

    ```xml
    <VolumeRange Name="currentWeek">
        <RangeType>
            <Weekly WeekBegin="Monday" />
        </RangeType>
    </VolumeRange>
    ```

## `<BiMonthly>`

This splits the month in half so that there are two ranges.

| Attribute | Description |
|---------------|--------------|
| `SplitDay`  | The day of the month the split occurs on.

### Example

With the following example,  `SplitDay` of `15` is declared. This results in the first half of the month running the 1st–14th, and the second half starting on the 15th and running the rest of the month. This gives you two ranges in which you could pay commissions.

=== "XML"

    ```xml
    <VolumeRange Name="splitMonth">
        <RangeType>
            <BiMonthly SplitDay="15" />
        </RangeType>
    </VolumeRange>
    ```