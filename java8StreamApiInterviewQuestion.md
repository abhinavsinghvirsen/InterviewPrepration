# Java Streams api all Question combination like grouping highest salary ,2nd highest salary ,sum etc 

## Employee Class Definition

### let have class Employee & a list of employe
```java
class Employee {
    private int id;
    private String name;
    private int age;
    private long salary;
    private String gender;
    private String deptName;
    private String city;
    private int yearOfJoining;

    public Employee(int id, String name, int age, long salary, String gender, 
                    String deptName, String city, int yearOfJoining) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
        this.gender = gender;
        this.deptName = deptName;
        this.city = city;
        this.yearOfJoining = yearOfJoining;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", salary=" + salary +
                ", gender='" + gender + '\'' +
                ", deptName='" + deptName + '\'' +
                ", city='" + city + '\'' +
                ", yearOfJoining='" + yearOfJoining + '\'' +
                '}';
    }

    // Getters and Setters
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public long getSalary() {
        return salary;
    }

    public void setSalary(long salary) {
        this.salary = salary;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public String getDeptName() {
        return deptName;
    }

    public void setDeptName(String deptName) {
        this.deptName = deptName;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public int getYearOfJoining() {
        return yearOfJoining;
    }

    public void setYearOfJoining(int yearOfJoining) {
        this.yearOfJoining = yearOfJoining;
    }
}


List<Employee> employeeList = new ArrayList<>();
employeeList.add(new Employee(1, "abc", 28, 123, "F", "HR", "Blore", 2020));
employeeList.add(new Employee(2, "xyz", 29, 120, "F", "HR", "Hyderabad", 2015));
employeeList.add(new Employee(3, "efg", 30, 115, "M", "HR", "Chennai", 2014));
employeeList.add(new Employee(4, "def", 32, 125, "F", "HR", "Chennai", 2013));
employeeList.add(new Employee(5, "ijk", 22, 150, "F", "IT", "Noida", 2013));
employeeList.add(new Employee(6, "mno", 27, 140, "M", "IT", "Gurugram", 2017));
employeeList.add(new Employee(7, "uvw", 26, 130, "F", "IT", "Pune", 2016));
employeeList.add(new Employee(8, "pqr", 23, 145, "M", "IT", "Trivandam", 2015));
employeeList.add(new Employee(9, "stv", 25, 160, "M", "IT", "Blore", 2010));
```
### 1.Group the Employees by city
   
  ```java 
   Map<String, List<Employee>> employeesByCity;
   employeesByCity = employeeList.stream().collect(Collectors.groupingBy(Employee::getCity));
   System.out.println("Employees grouped by city :: \n" + employeesByCity);
```

### 2.Group the Employees by age.

```java

Map<Integer, List<Employee>> employeesByAge = employeeList.stream().collect(Collectors.groupingBy(Employee::getAge));
System.out.println("Employees grouped by age :: \n" + employeesByAge);
```

### 3.Find the count of male and female employees present in the organization.
```java
Map<String, Long> noOfMaleAndFemaleEmployees = employeeList.stream().collect(Collectors.groupingBy(Employee::getGender, Collectors.counting()));
System.out.println("Count of male and female employees present in the organization:: \n" + noOfMaleAndFemaleEmployees);
```
### 4.Print the names of all departments in the organization.

```java

System.out.println("Names of all departments in the organization ");
employeeList.stream().map(Employee::getDeptName).distinct().forEach(System.out::println);
```

### 5.Print employee details whose age is greater than 28.

```java

System.out.println("Employee details whose age is greater than 28");
employeeList.stream().filter(e -> e.getAge() > 28).collect(Collectors.toList()).forEach(System.out::println);
```

### 6.Find maximum age of employee.

```java
OptionalInt max = employeeList.stream().mapToInt(Employee::getAge).max();
if (max.isPresent()) 
    System.out.println("Maximum age of Employee: " + max.getAsInt());
```

### 7.Print Average age of Male and Female Employees.

```java
Map<String, Double> avgAge = employeeList.stream().collect(Collectors.groupingBy(Employee::getGender, Collectors.averagingInt(Employee::getAge)));
System.out.println("Average age of Male and Female Employees:: " + avgAge);
```

### 8.Print the number of employees in each department.

```java
Map<String, Long> countByDept = employeeList.stream().collect(Collectors.groupingBy(Employee::getDeptName, Collectors.counting()));
System.out.println("No of employees in each department");
for (Map.Entry<String, Long> entry : countByDept.entrySet()) {
    System.out.println(entry.getKey() + " : " + entry.getValue());
} ```
### 9.Find oldest employee.

```java
Optional<Employee> oldestEmp = employeeList.stream().max(Comparator.comparingInt(Employee::getAge));
Employee oldestEmployee = oldestEmp.get();
System.out.println("Oldest employee details:: \n" + oldestEmployee);
```
### 10.Find youngest female employee.

```java

Optional<Employee> youngestEmp = employeeList.stream().filter(e -> e.getGender().equals("F")).min(Comparator.comparingInt(Employee::getAge));
Employee youngestEmployee = youngestEmp.get();
System.out.println("Youngest Female employee details:: \n" + youngestEmployee);
```
### 11.Find employees whose age is greater than 30 and less than 30.

```java
System.out.println("Employees whose age is greater than 25 and less than 25");
Map<Boolean, List<Employee>> partitionEmployeesByAge = employeeList.stream().collect(Collectors.partitioningBy(e -> e.getAge() > 30));

Set<Map.Entry<Boolean, List<Employee>>> empSet = partitionEmployeesByAge.entrySet();

for (Map.Entry<Boolean, List<Employee>> entry : empSet) {
    if (Boolean.TRUE.equals(entry.getKey())) {
        System.out.println("Employees greater than 30 years ::" + entry.getValue());
    } else {
        System.out.println("Employees less than 30 years ::" + entry.getValue());
    }
}```

### 12.Find the department name which has the highest number of employees.

```java
Map.Entry<String, Long> maxNoOfEmployeesInDept = employeeList.stream().collect(Collectors.groupingBy(Employee::getDeptName, Collectors.counting()))
                                                             .entrySet().stream().max(Map.Entry.comparingByValue()).get();
System.out.println("Max no of employees present in Dept :: " + maxNoOfEmployeesInDept.getKey());
```
### 13. Find if there any employees from HR Department.

```java
Optional<Employee> emp = employeeList.stream().filter(e -> e.getDeptName().equalsIgnoreCase("HR")).findAny();
emp.ifPresent(employee -> System.out.println("Found employees from HR department " + employee));
```
### 14.Find the department names that these employees work for, where the number of employees in the department is over 3.

```java

System.out.println("Department names where the number of employees in the department is over 3 :: ");
employeeList.stream().collect(Collectors.groupingBy(Employee::getDeptName, Collectors.counting()))
           .entrySet().stream().filter(entry -> entry.getValue() > 3).forEach(System.out::println);
```
### 15.Find distinct department names that employees work for.

```java

System.out.println("Distinct department names that employees work for:: ");
employeeList.stream().map(Employee::getDeptName).distinct().forEach(System.out::println);
```
### 16.Find all employees who lives in ‘Blore’ city, sort them by their name and print the names of employees.

```java
employeeList.stream().filter(e -> e.getCity().equalsIgnoreCase("Blore")).sorted(Comparator.comparing(Employee::getName)).forEach(e -> System.out.println("Employees staying in Blore:: " + e.getName()));
```
### 17.Number of employees in the organization.

```java
System.out.println("No of employees in the organisation :: " + employeeList.stream().count());
```
### 18.Find employee count in every department.

```java
Map<String, Long> employeeCountInDepartmentMap = employeeList.stream().collect(Collectors.groupingBy(Employee::getDeptName, Collectors.counting()));
System.out.print("Employee department and its count :- \n" + employeeCountInDepartmentMap);
```
### 19.Find the department which has the highest number of employees.

```java
Optional<Map.Entry<String, Long>> deptNameWithHighestEmp = employeeCountInDepartmentMap.entrySet().stream().max(Map.Entry.comparingByValue());
if (deptNameWithHighestEmp.isPresent()) {
    System.out.println("Department which has the highest number of employees " + deptNameWithHighestEmp.get());
}
```

### 20.Sorting a Stream by age and name fields.

```java
System.out.println("Sorting based on name and age:: ");
Comparator<Employee> comparator1 = Comparator.comparing(Employee::getName);
Comparator<Employee> comparator2 = Comparator.comparingInt(Employee::getAge);
employeeList.stream().sorted(comparator1.thenComparing(comparator2)).forEach(System.out::println);
```
### 21. Highest experienced employees in the organization.

```java
Optional<Employee> seniorEmp = employeeList.stream().sorted(Comparator.comparingInt(Employee::getYearOfJoining)).findFirst();
System.out.println("Senior Employee Details :" + seniorEmp.get());
```
### 22.Print average and total salary of the organization.

```java
DoubleSummaryStatistics empSalary = employeeList.stream().collect(Collectors.summarizingDouble(Employee::getSalary));

System.out.println("Average Salary in the organisation = " + empSalary.getAverage());
System.out.println("Total Salary in the organisation  = " + empSalary.getSum());
```
### 23.Print Average salary of each department.

```java

System.out.println("Print Average salary of each department");
Map<String, Double> avgSalary = employeeList.stream().collect(Collectors.groupingBy(Employee::getDeptName, Collectors.averagingDouble(Employee::getSalary)));
Set<Map.Entry<String, Double>> entrySet = avgSalary.entrySet();
for (Map.Entry<String, Double> entry : entrySet) {
    System.out.println(entry.getKey() + " : " + entry.getValue());
}
```

### 24.Find Highest salary in the organisation.

```java
Optional<Employee> empHighest = employeeList.stream().sorted(Comparator.comparingDouble(Employee::getSalary).reversed()).findFirst();
System.out.println("Highest Salary in the organisation : " + empHighest.get().getSalary());
```
### 25.Find Second Highest salary in the organisation.

```java

Optional<Employee> emp2 = employeeList.stream().sorted(Comparator.comparingDouble(Employee::getSalary).reversed()).skip(1).findFirst();
System.out.println("Second Highest Salary in the organisation : " + emp2.get().getSalary());
```

### 26.Nth Highest salary.

```java
int n = 10;// this can be any nth number highest salary
Optional<Employee> emp2 = empList.stream().sorted(Comparator.comparingDouble(Employee::getSalary)
                         .reversed()).skip(n-1).findFirst();
System.out.println("Second Highest Salary in the organisation : " + emp2.get().getSalary());
```


### 27. Find highest paid salary in the organization based on gender.

```java
Map<String, Optional<Employee>> highestPaidMFEmployee = empList.stream().collect(Collectors.groupingBy(Employee::getGender, 
                                                        Collectors.maxBy((t1, t2) -> (int) (t1.getSalary() - t2.getSalary()))));
System.out.println("Highest paid male and female employee : " + highestPaidMFEmployee);
```

### 28. Find lowest paid salary in the organization based on gender.

```java
Map<String, Optional<Employee>> lowestPaidMFEmployee = empList.stream().collect(Collectors.groupingBy(Employee::getGender, 
                                                       Collectors.minBy((t1, t2) -> (int) (t1.getSalary() - t2.getSalary()))));
System.out.println("Lowest paid male and female employee : " + lowestPaidMFEmployee);
```

### 29. Sort the employees' salaries in the organization in ascending order.

```java
System.out.println("Sorting the organization's employee salary in ascending order ");
empList.stream().sorted(Comparator.comparingLong(Employee::getSalary)).toList().forEach(System.out::println);
```

### 30. Sort the employees' salaries in the organization in descending order.

```java
System.out.println("Sorting the organization's employee salary in descending order ");
empList.stream().sorted(Comparator.comparingLong(Employee::getSalary).reversed()).toList().forEach(System.out::println);
```

### 31. Highest salary based on department.

```java
System.out.println("Highest salary dept wise:: \n" + empList.stream().collect(Collectors.groupingBy(Employee::getDeptName, Collectors.collectingAndThen(Collectors.toList(),
list -> list.stream().max(Comparator.comparingDouble(Employee::getSalary))))));
```

### 32. Print the second highest salary record based on department.

```java
System.out.println("Second highest salary dept wise:: \n" + empList.stream().collect(Collectors.groupingBy(Employee::getDeptName,
                   Collectors.collectingAndThen(Collectors.toList(),
                   list -> list.stream().sorted(Comparator.comparingDouble(Employee::getSalary).reversed()).skip(1).findFirst()))));
```

### 33. Sort the employees' salaries in each department in ascending order.

```java
System.out.println("Sorting the department wise employee salary in ascending order:: ");
Map<String, Stream<Employee>> sortedEmployeeAsc = empList.stream().collect(Collectors.groupingBy(Employee::getDeptName, 
                                                   Collectors.collectingAndThen(Collectors.toList(),
                                                   list -> list.stream().sorted(Comparator.comparingDouble(Employee::getSalary)))));

sortedEmployeeAsc.forEach((k, v) -> {
    System.out.println(k);
    System.out.println(v.collect(Collectors.toList()));
});
```

### 34. Sort the employees' salaries in each department in descending order.

```java
System.out.println("Sorting the department wise employee salary in descending order ");
Map<String, Stream<Employee>> sortedEmployeeDesc = empList.stream().collect(Collectors.groupingBy(Employee::getDeptName, 
                                                            Collectors.collectingAndThen(Collectors.toList(),
                                                            list -> list.stream().sorted(Comparator.comparingDouble(Employee::getSalary).reversed()))));

sortedEmployeeDesc.forEach((k, v) -> {
    System.out.println(k);
    System.out.println(v.collect(Collectors.toList()));
});
```

### 35. Find the employee with the highest salary increase over the years within each department.

This problem involves finding the employee who has received the highest salary increase over the years within each department. To solve this, we need to keep track of the salary history of each employee.

Assume we have another class `SalaryRecord` that tracks the salary changes of employees:

```java
class SalaryRecord {
    private int employeeId;
    private int year;
    private long salary;

    public SalaryRecord(int employeeId, int year, long salary) {
        this.employeeId = employeeId;
        this.year = year;
        this.salary = salary;
    }

    public int getEmployeeId() {
        return employeeId;
    }

    public int getYear() {
        return year;
    }

    public long getSalary() {
        return salary;
    }
}
```

The `Employee` class should have an additional method to get a list of `SalaryRecord` objects for each employee:

```java
public List<SalaryRecord> getSalaryRecords() {
    // Implementation to return the list of salary records for the employee
}
```

Here is how you can solve the problem:

```java
// Assuming empList contains the list of employees and salaryRecords contains the list of salary records
List<SalaryRecord> salaryRecords = Arrays.asList(
    new SalaryRecord(1, 2020, 1000),
    new SalaryRecord(1, 2021, 1500),
    new SalaryRecord(2, 2020, 1200),
    new SalaryRecord(2, 2021, 1400),
    // Add other records
);

Map<String, Employee> highestSalaryIncreaseByDept = empList.stream()
    .collect(Collectors.groupingBy(Employee::getDeptName))
    .entrySet().stream()
    .collect(Collectors.toMap(
        Map.Entry::getKey,
        entry -> entry.getValue().stream()
            .max(Comparator.comparingLong(emp -> {
                List<SalaryRecord> records = salaryRecords.stream()
                    .filter(record -> record.getEmployeeId() == emp.getId())
                    .sorted(Comparator.comparingInt(SalaryRecord::getYear))
                    .collect(Collectors.toList());
                
                if (records.size() < 2) return 0L; // Not enough data for comparison

                long initialSalary = records.get(0).getSalary();
                long latestSalary = records.get(records.size() - 1).getSalary();

                return latestSalary - initialSalary;
            }))
            .orElse(null)
    ));

System.out.println("Employee with highest salary increase by department: " + highestSalaryIncreaseByDept);
```

This code snippet demonstrates how to:
1. Group employees by department.
2. Track salary changes for each employee over the years.
3. Calculate the salary increase for each employee.
4. Find the employee with the highest salary increase in each department.
