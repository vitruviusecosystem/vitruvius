# Vitruvius

**Autodesk Revit add-ins for the AEC industry.**

Vitruvius builds commercial add-ins for Autodesk Revit. Our first product
automatically repairs reversed and garbled Hebrew text in linked DWG/DXF
drawings — a long-standing problem for Israeli and RTL-language architecture
offices, where consultant CAD files render as gibberish inside Revit.

## What it does
- Detects EN-typed and legacy-encoded Hebrew in linked CAD files
- Reverses and re-encodes the text so it displays correctly (right-to-left)
- Manages SHX → TTF font mapping
- Auto-reloads the links so drawings become readable with a single click

## Tech
Revit .NET API (Revit 2024) · AutoCAD core / DXF processing · C#

## Status
Private beta with an Israeli architecture firm.
