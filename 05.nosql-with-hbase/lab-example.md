Advanced HBase Tutorial for Students

This tutorial covers a range of operations in HBase from basic table and data management to more advanced features like filters, time-based data handling, and an introduction to coprocessors.

## Use Case: School System Database

### Initial Setup

- **Create a Namespace**
  
  ```shell
  create_namespace 'SchoolSystem'
  ```
  
- **Create Tables and Column Families**
  
  - Student Information and Grades
    
    ```shell
    create 'SchoolSystem:StudentInfo', 'details', 'grades'
    ```
    
  - Attendance Records
    
    ```shell
    create 'SchoolSystem:Attendance', {NAME => 'daily', VERSIONS => 3}
    ```
    

### Managing Data

- **Insert Student Information**
  
  ```shell
  put 'SchoolSystem:StudentInfo', 'student1', 'details:name', 'John Doe'
  put 'SchoolSystem:StudentInfo', 'student1', 'details:email', 'john.doe@example.com'
  ```
  
- **Insert Grades and Update Versions**
  
  ```shell
  put 'SchoolSystem:StudentInfo', 'student1', 'grades:Math', 'A'
  put 'SchoolSystem:StudentInfo', 'student1', 'grades:Science', 'B+'
  ```
  
  Update and keep multiple versions of a grade:
  
  ```shell
  alter 'SchoolSystem:StudentInfo', NAME => 'grades', VERSIONS => 3
  put 'SchoolSystem:StudentInfo', 'student1', 'grades:Science', 'A-'
  ```
  
- **Track Attendance with Timestamps**
  
  ```shell
  put 'SchoolSystem:Attendance', 'student1_20230101', 'daily:status', 'Present'
  put 'SchoolSystem:Attendance', 'student1_20230102', 'daily:status', 'Absent'
  ```
  

### Querying Data

- **Retrieve Data with Filters**
  
  Filter by prefix to get attendance records for a student:
  
  ```shell
  scan 'SchoolSystem:Attendance', {FILTER => "PrefixFilter('student1')"}
  ```
  
- **Retrieve Multiple Versions of Data**
  
  Get multiple versions of the Science grade for `student1`:
  
  ```shell
  scan 'SchoolSystem:StudentInfo', {COLUMNS => ['grades:Science'], VERSIONS => 2}
  ```
  

### Advanced Data Management in HBase

#### Step 3: Import Data into HBase

After setting up your tables and column families, you might want to bulk import data into HBase. This can be efficiently done using the `ImportTsv` tool, which allows for importing tab-separated values (TSV) files directly into an HBase table.

Exit the HBase shell to execute shell commands:

```shell
exit
```

Run the following command to import data from a TSV file into the `SchoolSystem:StudentInfo` table:

```shell
hbase org.apache.hadoop.hbase.mapreduce.ImportTsv   -Dimporttsv.separator=","   -Dimporttsv.columns="HBASE_ROW_KEY,details:name,details:email,grades:Math,grades:Science"   'SchoolSystem:StudentInfo' /path/to/your/data.tsv
```

**Note**: Replace `/path/to/your/data.tsv` with the actual file path.

Log back into the HBase shell:

```shell
hbase shell
```

Verify the imported data:

```shell
scan 'SchoolSystem:StudentInfo', {LIMIT => 10}
```

### Apply Filters

HBase provides several filters to refine your data retrieval operations, enabling efficient queries without full table scans.

#### Filter Example

Let's say we want to display `grades:Science` for students, but only for those where `details:email` is defined:

```shell
import org.apache.hadoop.hbase.filter.CompareFilter
import org.apache.hadoop.hbase.filter.SingleColumnValueFilter
import org.apache.hadoop.hbase.filter.BinaryComparator
import org.apache.hadoop.hbase.util.Bytes

scan 'SchoolSystem:StudentInfo', {
  COLUMNS => 'grades:Science',
  LIMIT => 10,
  FILTER => SingleColumnValueFilter.new(
    Bytes.toBytes('details'), 
    Bytes.toBytes('email'),
    CompareFilter::CompareOp.valueOf('NOT_EQUAL'),
    BinaryComparator.new(Bytes.toBytes(''))
  )
}
```

### Cleanup

- **Disable and Drop a Table**
  
  ```shell
  disable 'SchoolSystem:StudentInfo'
  drop 'SchoolSystem:StudentInfo'
  ```
