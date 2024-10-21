# Composite 组合模式
>what(是什么): 将一组对象组织（Compose）成树形结构，以表示一种“部分 - 整体”的层次结构。组合让客户端（在很多设计模式书籍中，“客户端”代指代码的使用者。）可以统一单个对象和组合对象的处理逻辑。<br>
>when(什么时候用): 当数据满足树型结构时，例如文件系统中文件路径，公司系统多层级关系<br>
>why(为什么用)：简化树形结构中对象的处理，无论它们是单个对象还是组合对象。<br>
>how(如何用):<br>

个人理解：对于组合设计模式其实更像数据结构和算法的抽象然后应用于业务中，处理树形结构的数据。



``` 
<!-- 组合模式 -->
<!-- 
Component接口：定义了所有对象必须实现的操作。
Leaf类：实现Component接口，代表树中的叶子节点。
Composite类：也实现Component接口，并包含其他Component对象的集合。 -->

public abstract class HumanResource {
  protected long id;
  protected double salary;

  public HumanResource(long id) {
    this.id = id;
  }

  public long getId() {
    return id;
  }

  public abstract double calculateSalary();
}

public class Employee extends HumanResource {
  public Employee(long id, double salary) {
    super(id);
    this.salary = salary;
  }

  @Override
  public double calculateSalary() {
    return salary;
  }
}

public class Department extends HumanResource {
  private List<HumanResource> subNodes = new ArrayList<>();

  public Department(long id) {
    super(id);
  }

  @Override
  public double calculateSalary() {
    double totalSalary = 0;
    for (HumanResource hr : subNodes) {
      totalSalary += hr.calculateSalary();
    }
    this.salary = totalSalary;
    return totalSalary;
  }

  public void addSubNode(HumanResource hr) {
    subNodes.add(hr);
  }
}

// 构建组织架构的代码
public class Demo {
  private static final long ORGANIZATION_ROOT_ID = 1001;
  private DepartmentRepo departmentRepo; // 依赖注入
  private EmployeeRepo employeeRepo; // 依赖注入

  public void buildOrganization() {
    Department rootDepartment = new Department(ORGANIZATION_ROOT_ID);
    buildOrganization(rootDepartment);
  }

  private void buildOrganization(Department department) {
    List<Long> subDepartmentIds = departmentRepo.getSubDepartmentIds(department.getId());
    for (Long subDepartmentId : subDepartmentIds) {
      Department subDepartment = new Department(subDepartmentId);
      department.addSubNode(subDepartment);
      buildOrganization(subDepartment);
    }
    List<Long> employeeIds = employeeRepo.getDepartmentEmployeeIds(department.getId());
    for (Long employeeId : employeeIds) {
      double salary = employeeRepo.getEmployeeSalary(employeeId);
      department.addSubNode(new Employee(employeeId, salary));
    }
  }
}

```



