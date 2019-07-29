---
layout:     post
title:      "Unity Excel 文件读取和写入"
subtitle:   "Excel读写"
date:       2018-05-10
author:     "Passion"
header-img: "img/home-bg-o.jpg"
tags:
    - unity
    - excel
---

## 一.前言
在网上看到很多Unity 的解析Excel 的文章

但是在使用的过程中还是碰到了不少的问题，在这里总结一下，希望能对看到此处的朋友一个帮助。

## 二.Excel的读取
需要加入库文件 Excel.dll 和ICSharpCode.SharpZipLib库文件，官方链接 [http://exceldatareader.codeplex.com/](http://exceldatareader.codeplex.com/)


#### 需要添加的命名空间
`using Excel;`

#### 读取方法
```cs
using UnityEngine;
using Excel;
using System.Data;
using System.IO;
using System.Collections.Generic;
 
public class ExcelAccess
{
    public static string Excel = "Book";
 
    //查询menu表
    public static List<Menu> SelectMenuTable()
    {
        string excelName = Excel + ".xlsx";
        string sheetName = "sheet1";
        DataRowCollection collect = ExcelAccess.ReadExcel(excelName, sheetName);
 
        List<Menu> menuArray = new List<Menu>();
        for (int i = 1; i < collect.Count; i++)
        {
            if (collect[i][1].ToString() == "") continue;
 
            Menu menu = new Menu
            {
                m_Id = collect[i][0].ToString(),
                m_level = collect[i][1].ToString(),
                m_parentId = collect[i][2].ToString(),
                m_name = collect[i][3].ToString()
            };
            menuArray.Add(menu);
        }
        return menuArray;
    }
 
    /// <summary>
    /// 读取 Excel ; 需要添加 Excel.dll; System.Data.dll;
    /// </summary>
    /// <param name="excelName">excel文件名</param>
    /// <param name="sheetName">sheet名称</param>
    /// <returns>DataRow的集合</returns>
    static DataRowCollection ReadExcel(string excelName,string sheetName)
    {
        string path= Application.dataPath + "/" + excelName;
        FileStream stream = File.Open(path, FileMode.Open, FileAccess.Read, FileShare.Read);
        IExcelDataReader excelReader = ExcelReaderFactory.CreateOpenXmlReader(stream);
 
        DataSet result = excelReader.AsDataSet();
        //int columns = result.Tables[0].Columns.Count;
        //int rows = result.Tables[0].Rows.Count;
 
        //tables可以按照sheet名获取，也可以按照sheet索引获取
        //return result.Tables[0].Rows;
        return result.Tables[sheetName].Rows;
    }
}
```

#### 注意：
这里逻辑很简单，如果有不懂得可以上Excel的文档里去看，但是这个Excel的库有一个限制，就是只能读不能写，并且只能在编辑器下用，如果打包出来加载时会报空指针异常，原因就不清楚了。

所以建议大家，让策划把Excel写好后，在编辑器下读取后用Unity 的ScriptableObject 类保存成Asset文件，可以在运行时更方便的读取。


好了，Excel的读取就到这里。接下来讲一下Excel 的写入，怎么生成一个Excel文件，并把数组或字典中的数据写入Excel中呢？

## 三.Excel 的写入
此时需要一个Excel.dll的姐妹，EPPlus.dll 官方链接 [https://epplus.codeplex.com/releases/view/118053](https://epplus.codeplex.com/releases/view/118053)

使用方法在官方的文档中都有，这里只贴出我的实现方式。

#### 需要添加的命名空间
`using OfficeOpenXml;`

#### 写入方法
```cs
    /// <summary>
    /// 写入 Excel ; 需要添加 OfficeOpenXml.dll;
    /// </summary>
    /// <param name="excelName">excel文件名</param>
    /// <param name="sheetName">sheet名称</param>
    public static void WriteExcel(string excelName, string sheetName)
    {
        //通过面板设置excel路径
        //string outputDir = EditorUtility.SaveFilePanel("Save Excel", "", "New Resource", "xlsx");
 
        //自定义excel的路径
        string path = Application.dataPath + "/" + excelName;
        FileInfo newFile = new FileInfo(path);
        if (newFile.Exists)
        {
            //创建一个新的excel文件
            newFile.Delete();
            newFile = new FileInfo(path);
        }
 
        //通过ExcelPackage打开文件
        using (ExcelPackage package = new ExcelPackage(newFile))
        {
            //在excel空文件添加新sheet
            ExcelWorksheet worksheet = package.Workbook.Worksheets.Add(sheetName);
            //添加列名
            worksheet.Cells[1, 1].Value = "ID";
            worksheet.Cells[1, 2].Value = "Product";
            worksheet.Cells[1, 3].Value = "Quantity";
            worksheet.Cells[1, 4].Value = "Price";
            worksheet.Cells[1, 5].Value = "Value";
 
            //添加一行数据
            worksheet.Cells["A2"].Value = 12001;
            worksheet.Cells["B2"].Value = "Nails";
            worksheet.Cells["C2"].Value = 37;
            worksheet.Cells["D2"].Value = 3.99;
            //添加一行数据
            worksheet.Cells["A3"].Value = 12002;
            worksheet.Cells["B3"].Value = "Hammer";
            worksheet.Cells["C3"].Value = 5;
            worksheet.Cells["D3"].Value = 12.10;
            //添加一行数据
            worksheet.Cells["A4"].Value = 12003;
            worksheet.Cells["B4"].Value = "Saw";
            worksheet.Cells["C4"].Value = 12;
            worksheet.Cells["D4"].Value = 15.37;
 
            //保存excel
            package.Save();
        }
    }
```

把上面的数据换成你自己的数组和字典遍历就OK了。欢迎大神指教。

由于大家遇到问题较多，特附上工程地址
Git地址：[https://git.oschina.net/passionyu/ExcelReadWrite.git](https://git.oschina.net/passionyu/ExcelReadWrite.git)
