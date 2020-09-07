# Case Study 1

1. Find the ids of all documents which do not have any pages.

```
objectarraywithwhere = Document.joins(:pages).select('documents.id, COUNT(pages.*) AS pages_count').group('documents.id').pluck('documents.id')
foundids = Document.where.not(id: objectarraywithwhere).pluck('documents.id')
```

2. Return a list of report titles and the total number of pages in the report. Reports which do not have pages may be ignored.

```
records = Report.joins(documents: :pages).select('reports.*, COUNT(pages.*) AS pages_count').group('reports.id')
table = []
records.each do |record|
    table.push({title: record.title, pages_count:record.pages_count})
end
```


3. How would you determine the percentage of document pages which include a footnote?

Assuming 'document' refers to a record in the document table:

```
numerator = Page.all.where(document_id: document.id).where.not(footnote: nil).count
denominator = Document.pages.count
percentage = (numerator / denominator) * 100%
```

4. How would you search the body of a page to look for a specific search string? Any approach is welcome, though you may only use native methods, not gems or other libraries.

Assuming 'string' is the specific search string to look for:
```
Page.all.select { |page| page.body.include?(string)}.count
```

# Case Study 2

1. Return an array of fund objects in alphabetical order.
```
function returnFundObjects() {
    fundListArray = []
    fundList = Object.values(entities.fund)
    fundListSorted = fundList.sort(function(a, b){
        if(a.name > b.name){return 1};
        if(a.name < b.name){return -1};
        return 0
    })
    return fundListSorted
}
```

 2. Return an array of companies that belong to fund 2.

```
function returnFund2Companies(){
    return Object.values(entities.company).filter(company => 
        company.fund_id == 2
    )
}
```

3. Return an array of fund names with an exited company.

```
function fundsWithExitedCo(){
    // Create an array of fund_ids from companies that have exited
    exitedCompanies = []
    Object.values(entities.company).map(company => {
        if(company.exited === true){
            exitedCompanies.push(company.fund_id)
        }
    })

    // Create an array with fund names
    fundArray = []
    Object.values(entities.fund).map(fund => {
        if(exitedCompanies.includes(fund.id)){
            fundArray.push(fund.name)
        }
    })

    return fundArray
}
```
