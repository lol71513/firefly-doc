﻿# Exercise - result

The **ShowProducts** class should look like :
```csdiff
using System;
using System.Collections.Generic;
using System.Text;
using System.Drawing;
using Firefly.Box;
using ENV;
using ENV.Data;

namespace Northwind.Exercises
{
    public class ShowProducts : UIControllerBase
    {

        public readonly Models.Products Products = new Models.Products();
        public readonly Models.Categories Categories = new Models.Categories();

        public ShowProducts()
        {
            From = Products;
            Relations.Add(Categories, RelationType.Find,
                Categories.CategoryID.IsEqualTo(Products.CategoryID));

            Where.Add(Products.CategoryID.IsEqualTo(2).Or(Products.CategoryID.IsEqualTo(4).Or(Products.CategoryID.IsEqualTo(6))));
            Where.Add(Products.UnitPrice.IsGreaterThan(25));

            OrderBy = Products.SortByProductName;
        }

        public void Run()
        {
            Execute();
        }

        protected override void OnLoad()
        {
            View = () => new Views.ShowProductsView(this);
        }
        protected override void OnSavingRow()
        {
            if (Products.ProductName == "")
                Message.ShowError("Product name is empty!");
            if (Products.UnitsInStock == 0 || Products.UnitsOnOrder == 0)
                System.Windows.Forms.MessageBox.Show("Units is zero");
            if (Products.UnitsInStock < 10)
                Message.ShowWarning("Not enough units");
            if (Products.UnitsOnOrder >= 50 && Products.UnitsOnOrder <= 100)
                Message.ShowError("Units on order are between 50 and 100");
+           if (u.Len(u.Trim(Products.ProductName)) < 3)
+               Message.ShowError("Please enter a valid product name");
+           if (u.InStr(Products.ProductName,"%")>=1|| u.InStr(Products.ProductName, "@") >= 1)
+               Message.ShowError("% and @ are invalid characters in product name");
+           if (u.Left(Products.ProductName,1)=="T")
+               Message.ShowWarningInStatusBar("Product name can't start with a T");
        }
    }
}
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/FQNAVNcdtTo?list=PL1DEQjXG2xnL1VKb5GvdDwxJeym7Uj6S3" frameborder="0" allowfullscreen></iframe>