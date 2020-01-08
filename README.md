# automock
轻量级全自动mock

只需要一个jar包，全自动完成java的mock操作，使用极其简单

只需要一行代码直接调用

/**
   * mock 一个类
   *
   * @param fullPkg 全限定的包名+类名：如 com.mht.auto.mock.AutoMock
   */
AutoMock.mock(pkg);


  /**
   * mock 包下所有类
   *
   * @param fullPkg 全限定的包名：如 com.mht.auto.mock
   */
AutoMock.mockPkg(pkg);
============================================================================以下都是效果和说明
执行输出效果：
***

   _   _   _   _   _   _   _   _  
  / \ / \ / \ / \ / \ / \ / \ / \ 
 ( A ( u ( t ( o ( m ( o ( c ( k )
  \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ 
  
欢迎使用AutoMock自动化工具，本工具可生成mock测试类并加载执行：

mock测试类生成位于src/test/java/mock目录下，如有自定义改动请自取。

本次执行：

***
-----[SchedulerJobServiceImpl].[releaseJobs] start......
|													|
|													|
-----[SchedulerJobServiceImpl].[releaseJobs] mock success!


-----[SchedulerJobServiceImpl].[runJob] start......
|													|
|													|
-----[SchedulerJobServiceImpl].[runJob] mock success!


***

项目结构必须是：
src
 main
  java
 test
  java

运行过程：
1   自动读取java文件
2   自动根据原文件生成mock测试类文件
3   自动格式化写出测试类文件
4   自动热加载文件，并执行mock的方法

生成的mock文件样例：

***

package mock;

import static org.mockito.Mockito.any;
import static org.mockito.Mockito.anyInt;
import static org.mockito.Mockito.when;

import com.core.util.DataUtil;
import com.test.server.test.easy.vo.Result;
import org.junit.Before;
import org.junit.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

public class TestJobServiceImplTest {
  @InjectMocks TestJobServiceImpl testJobServiceImpl = new TestJobServiceImpl();
  @Mock TestService testService;

  @Before
  public void setUp() {
    MockitoAnnotations.initMocks(this);
  }

  @Test
  public void releaseJobs() {
    String projectName = DataUtil.build(String.class);
    String jobName = DataUtil.build(String.class);
    int online = 1;
    Result result = DataUtil.build(Result.class);
    when(this.processDefinitionService.queryProcessDefinitionListPaging(
            any(), any(), anyInt(), anyInt(), any()))
        .thenReturn(result);
    Result scheduleResult = DataUtil.build(Result.class);
    when(this.testService.queryTest(any(), anyInt(), any(), anyInt(), anyInt()))
        .thenReturn(scheduleResult);
    when(this.testService.setTestStateOffline(any(), anyInt())).thenReturn(null);
    when(this.testService.setTestState(any(), anyInt())).thenReturn(null);
    this.testJobServiceImpl.releaseJobs(projectName, jobName, online);
  }
}

***

可直接运行，有自定义mock操作可直接修改即可。使用无比方便，一键批量生成。
