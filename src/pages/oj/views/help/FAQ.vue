<template>
  <panel>
    <div slot="title">{{$t('m.Frequently_Asked_Questions')}}</div>
    <div class="content markdown-body">
      <ul>
        <li>输入输出在哪里？
          <p>你的程序应该从 <code>stdin</code>(标准输入) 获得输入，把输出写到 <code>stdout</code>(标准输出)中。
            例如,C语言可以使用 <code>scanf</code>从stdin读取输入,用 <code>printf</code> 写入到stdout中。 C++可以使用 <code>cin</code>和 <code>cout</code> 。
            除非题目要求使用文件I/O，否则你的程序不允许读取或写入文件, oj将会返回 <code>Runtime Error</code>。
          </p>
        </li>
        <li>提交中的Time的含义是什么？
        <p>
          oj可能会使用不同的输入文件多次测试代码。 如果代码在每个输入文件的时间限制内给出正确的答案，则显示的执行时间Time是每个测试用例花费的最大时间。 否则，执行时间就没有意义了。
        </p>
        </li>
        <li>如何在C++中使用int64？
          <p>用 <code>long long</code> 声明，用<code>cin/cout</code> 或者 <code>%lld</code>输入输出， 用<code> __int64</code> 会导致<code>Compile Error</code>。</p>
        </li>
        <li>Java/Scala/Kotlin的规范？
          <p>所有的程序都必须实现名为 <code>Main</code> 的类（对于Kotlin，不要设置自定义的类名）。不要使用其他public类。 使用加速的I/O，例如 <code>BufferedReader</code> 以避免Time Limit Exceeded。</p>
        </li>
        <li>Presentation Error（格式错误）？
          <p>已实现。
           有些题目可能设置了忽略格式的判题方式，返回的就会是<code>Accepted</code>。</p>
        </li>
        <li>如何报告oj的bug？
          <p>
            有问题直接联系<a href="mailto:ccw630@gmail.com">管理员</a>.<br/>
            <s style="opacity:0;">The onlinejudge is open source, you can open an issue in <a href="https://github.com/custoj/CustOJ-Backend">Github</a>.
            The details(like env, version..) about a bug is required, which will help us a lot to solve the bug.
            Certainly, we are very pleased to merge your pull requests.</s>
          </p>
        </li>
      </ul>
    </div>
  </panel>
</template>

<script>
</script>

<style lang="less" scoped>
  .content {
    font-size: 16px;
    margin: 0 50px 40px 50px;
    > ul {
      list-style: disc;
      li {
        font-size: 16px;
        margin-top: 20px;
        &:first-child {
          margin-top: 0;
        }
        p {
          font-size: 14px;
          margin-top: 5px;
        }
      }
    }
  }
</style>
