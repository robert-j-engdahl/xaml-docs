---
title: ExportToWorkbook
page_title: ExportToWorkbook
description: ExportToWorkbook
slug: gridview-export-workbook
tags: gridview,export,workbook
published: True
position: 3
---

# ExportToWorkbook

In __R1 2016__ we introduced a new extension method related to the exporting of RadGridView - *ExportToWorkbook()*. You can use it if you need to modify the content of the exported RadGridView and avoid styling the document manually.

## Assembly References

__ExportToWorkbook__ uses additional libraries so you need to add references to the following assemblies:

* Telerik.Windows.Controls.GridView.Export.dll
* Telerik.Windows.Documents.Core.dll
* Telerik.Windows.Documents.Spreadsheet.dll 
* Telerik.Windows.Documents.Spreadsheet.FormatProviders.OpenXml.dll
* Telerik.Windows.Zip.dll

## Usage

This method exports the associated RadGridView to a [Workbook](https://docs.telerik.com/devtools/document-processing/libraries/radspreadprocessing/working-with-workbooks/working-wtih-workbooks-what-is-workbook) object. **Examples 1 and 2** show how you can modify that object before exporting.

#### __[C#] Example 1: Export RadGridView to a Workbook and modify cell style:__
{{region cs-gridview-export-workbook-0}}
	  private void Button_Click(object sender, RoutedEventArgs e)
        {
			//Instantiate the Workbook object
            Workbook workbook = this.clubsGrid.ExportToWorkbook();

			//Modify the created Workbook
            CellStyle wbStyle = workbook.Styles["Normal"];
            wbStyle.ForeColor = new ThemableColor(Colors.Green);
            wbStyle.FontFamily = new ThemableFontFamily(ThemeFontType.Major);
            wbStyle.FontSize = UnitHelper.PointToDip(16);
            wbStyle.VerticalAlignment = RadVerticalAlignment.Top;

			//Export the Workbook to an Excel file
            SaveFileDialog dialog = new SaveFileDialog();
            dialog.DefaultExt = "*.xlsx";
            dialog.Filter = String.Format("{1} files (*.{0})|*.{0}|All files (*.*)|*.*", "xlsx", "Excel");
            dialog.FilterIndex = 1;

            if (dialog.ShowDialog() == true)
            {
                var provider = new XlsxFormatProvider();
                using (var output = dialog.OpenFile())
                {
                    provider.Export(workbook, output);
                }
            }
        }
{{endregion}}

#### __[C#] Example 2: Double the width of the exported columns:__
{{region cs-gridview-export-workbook-1}}
    for (int i = 0; i < workbook.ActiveWorksheet.UsedCellRange.ColumnCount; i++)
    {
        workbook.ActiveWorksheet.Columns[i].SetWidth(new ColumnWidth(this.clubsGrid.Columns[i].ActualWidth * 2, true));
    }
{{endregion}}

## GridViewDocumentExportOptions

The method can be overloaded and take __GridViewDocumentExportOptions__ as a parameter. You can use it to set the following export options:

* Culture
* Items
* ShowColumnFooters
* ShowGroupFooters
* ShowColumnHeaders
* ExportDefaultStyles  

>important The __ExportToWorkbook__ method utilizes the **SpreadProcessing library**. You can check the respective [documentation](https://docs.telerik.com/devtools/document-processing/libraries/radspreadprocessing/overview) for more information on how to use the library.

## See Also

* [SpreadsheetStreamingExport]({%slug gridview-export-spreadsheetstreamingexport%})
* [ExportToXlsx]({%slug gridview-export-xlsx%})
* [ExportToPdf]({%slug gridview-export-pdf%})