---
title       : Gene expression in acute leukemia
subtitle    : 
author      : Velizar Shivarov
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [bootstrap, quiz]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Motivation

1. Omics technologies are paving their way towards the clinics
2. Large scale data require in-depth analytical skills
3. User-friendly applications analytical tools are needed

## Goal
1. To develop a shiny application demonstrating differential gene expression in acute leukemia
2. To perform that task using a classical dataset from [Golub et al.(Science, 1999)](http://www.ncbi.nlm.nih.gov/pubmed/10521349)
3. Analysis of differentially expressed genes is performed the [Linear Models for Microarray Data (limma package for Bioconductor)](http://www.bioconductor.org/packages/release/bioc/html/limma.html) 

---  

## List of differentially expressed genes





```r
data(golub)
gol.fac<-factor(golub.cl, levels=c(0,1), labels=c("ALL","AML"))
design<-model.matrix(~gol.fac)
colnames(design)<-c("ALL","AML")
fit<-lmFit(golub,design)
fit2<-eBayes(fit)
tbl<-topTable(fit2, coef ="AML",adjust="BH", p.value=0.01, sort.by="P", number=5)
tbl
```

```
##         logFC     AveExpr         t      P.Value    adj.P.Val        B
## 829  2.891941  0.02669711 10.773364 1.230726e-13 3.754946e-10 20.74376
## 378  1.951224 -0.41686789  8.768583 5.070903e-11 7.735662e-08 15.01477
## 2124 1.881461  0.24984079  8.473752 1.280639e-10 1.302409e-07 14.12714
## 1009 2.385291 -0.60397211  8.169773 3.361285e-10 2.469511e-07 13.20128
## 2670 1.874821 -0.56360658  8.071110 4.606863e-10 2.469511e-07 12.89857
```

---

## Heatmap with unsupervised clustering


```r
bluered <- colorRampPalette(c("blue","white","red"))(256)
col.fac<-as.character(factor(gol.fac, levels=c("ALL","AML"), labels=c("red","blue")))
heatmap(golub[as.numeric(rownames(tbl)),], ColSideColors=col.fac,
        Rowv=NULL, Colv=NULL, labCol=F, labRow=rownames(tbl),col=bluered)
```

<img src="assets/fig/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />

--- &radio
## Question 1 

If you use the default settings of the [ShinyApp] (https://vshivarov.shinyapps.io/golub_shiny3/) what is the ID of the sixth gene in the table output?

1. 748
2. _1145_
3. 2030
4. 808
