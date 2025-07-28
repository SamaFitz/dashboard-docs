# Superstore Executive Overview  

[![View Dashboard](https://raw.githubusercontent.com/SamaFitz/samafitz/main/images/Superstore%20Executive%20Overview.jpeg)](https://public.tableau.com/app/profile/samantha.fitzsimmons/viz/SuperstoreDashboardRedesign/ExecutiveOverview)

### 📊 Purpose  
Executive-level view of sales performance across states, categories, sub-categories, and products.  

---

### 🔄 Interaction Flow  

1. **Click a State** → filters the **Metric by Category** bar chart  
2. **Click a Category** → reveals **Sub-Categories** in the same chart  
3. **Click a Sub-Category** → filters the **Metric by Product** bar chart  

---

### 🗂 Dependency Diagram  

```mermaid
graph TD
    StateChart[Metric by State] --> CategoryChart[Metric by Category → Sub-Category]
    CategoryChart --> ProductChart[Metric by Product]

    CategorySet[1.1 Category Set] --> CategoryHeader[1.2 Category Header]
    CategorySet --> SubCategoryLevel[1.3 Sub-Category Level]
    SubCategoryLevel --> SegmentLevel[1.5 Segment Level]
    SegmentLevel --> CurrentLevel[1.6 Current Level]
    CurrentLevel --> SubCategoryHeader[1.7 Sub-Category Header]

    CategoryChart --> SubCatHighlight[Calc: Sub-Category Highlight]
    SubCatHighlight --> SubCatIfStatement[Calc: Sub-Cat If Statement]
    SubCatHighlight --> IsSubCatEmpty[Calc: Is the Sub-Cat Highlight Set Empty?]



### 📊 Metric by Category → Sub-Category Drilldown  

**Fields on Rows (left → right):**  

- `Category`  
- `1.2 Category Header`  
- `1.3 Sub-Category Level`  
- `1.7 Sub-Category Header`  
- `1.5 Segment Level`  

**Field on Color:**  

- `1.1 Category Set`

**Calculation Details**
// 1.2 Category Header
IF [1.1 Category Set] THEN [Category]
ELSE "•"
END

// 1.3 Sub-Category Level
IF [1.1 Category Set] THEN [Sub-Category]
ELSE [Category]
END

// 1.5 Segment Level
IF [1.1 Category Set] THEN [Sub-Category]
ELSE [Category]
END

// 1.6 Current Level
IF [1.5 Segment Level] = [Sub-Category] THEN 'Sub-Category'
ELSEIF [1.5 Segment Level] = [Category] THEN 'Category'
END

// 1.7 Sub-Category Header
IF [1.6 Current Level] = 'Sub-Category' THEN ""
ELSEIF [1.1 Category Set] THEN [Sub-Category]
ELSE ""
END


### 📊 Metric by Product Drilldown  

**Fields on Rows (left → right):**  

- `Category`  
- `Product Name`  
- `1.4 Sub-Category Level Set`  
- `Calc: Sub-Cat If Statement`  
- `Calc: Is the Sub-Cat Highlight Set Empty?`  

**Field on Color:**  

- `Calc: Is the Sub-Cat Highlight Set Empty?`  

**Calculation Details:**  
// Sub-Category Highlight
[1.1 Category Set] AND [1.4 Sub-Category Level Set]

// Sub-Cat If Statement
MAX(
  IF [Calc: Sub-Category Highlight] THEN [Sub-Category] END
)

// Is the Sub-Cat Highlight Set Empty?
{ SUM(
    IF [1.4 Sub-Category Level Set] THEN 1 ELSE 0 END
  )
} = 0


### 🌴 Dependency Tree  

1.1 Category Set  
 ├─> 1.2 Category Header  
 ├─> 1.3 Sub-Category Level  
 │    ├─> 1.5 Segment Level  
 │    │    ├─> 1.6 Current Level  
 │    │    │    └─> 1.7 Sub-Category Header  
 │    └─> 1.4 Sub-Category Level Set  
 └─> Sub-Category Highlight  
      ├─> Sub-Cat If Statement  
      └─> Is the Sub-Cat Highlight Set Empty?








