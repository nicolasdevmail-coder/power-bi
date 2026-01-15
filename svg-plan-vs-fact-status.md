# SVG Measure for tracking Plan vs Fact Status 


<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e38bc169-1ce4-493c-9861-9e2717790e10" />


## DAX Code
```dax
SVG: Plan/Fact Status = 
VAR SvgWidth = 200 // Total canvas width
VAR SvgHeight = 30 // Total canvas height
VAR Margin = 15 // Left & right margin
VAR UsableWidth = SvgWidth - ( Margin * 2 ) // Auto-calculated: 200 - 30 = 170

VAR DotRadius = 8
VAR LineWidth = 4
VAR LineTop = 4
VAR LineBottom = SvgHeight - 4
VAR DotCenterY = SvgHeight / 2

// data values
VAR PlanVal =
    SELECTEDVALUE ( Test[Plan] )
VAR FactVal =
    SELECTEDVALUE ( Test[Fact] )

// x-axis scale based on data
VAR MinPlan =
    CALCULATE ( MIN ( Test[Plan] ), ALL ( Test ) )
VAR MaxPlan =
    CALCULATE ( MAX ( Test[Plan] ), ALL ( Test ) )
VAR MinFact =
    CALCULATE ( MIN ( Test[Fact] ), ALL ( Test ) )
VAR MaxFact =
    CALCULATE ( MAX ( Test[Fact] ), ALL ( Test ) )
VAR MinScale =
MIN ( MinPlan, MinFact )
VAR MaxScale =
MAX ( MaxPlan, MaxFact )

// elements positioning
VAR PlanPos =
    Margin + DIVIDE ( PlanVal - MinScale, MaxScale - MinScale, 0 ) * UsableWidth
VAR FactPos =
    Margin + DIVIDE ( FactVal - MinScale, MaxScale - MinScale, 0 ) * UsableWidth // colors
VAR DotColor =
    IF ( FactVal >= PlanVal, "#22a867", "#d93317" )
VAR LineColor = "#404955"

// svg part
VAR SVG =
    "data:image/svg+xml,
<svg xmlns='http://www.w3.org/2000/svg' width='"& SvgWidth & "' height='" & SvgHeight & "'>" & 
    "<line x1='" & FORMAT ( PlanPos, "0" ) & "' y1='" & LineTop & "' x2='" & FORMAT ( PlanPos, "0" ) & "' y2='" & LineBottom & "' stroke='" & LineColor & "' stroke-width='" & LineWidth & "'/>" & 
    "<circle cx='" & FORMAT ( FactPos, "0" ) & "' cy='" & DotCenterY & "' r='" & DotRadius & "' fill='" & DotColor & "'/>" & 
    "</svg>"

RETURN
    IF ( NOT ISBLANK ( PlanVal ) && NOT ISBLANK ( FactVal ), SVG, BLANK () )
```
