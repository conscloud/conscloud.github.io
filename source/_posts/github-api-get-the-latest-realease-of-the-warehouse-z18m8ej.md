<h1>github api获取仓库的最新realease</h1>
<p>​#github#​</p>
<h3>实现代码</h3>
<pre><code class="language-bash">wget -qO- -t1 -T2 &quot;https://api.github.com/repos/lhc70000/iina/releases/latest&quot; | grep &quot;tag_name&quot; | head -n 1 | awk -F &quot;:&quot; '{print $2}' | sed 's/\&quot;//g;s/,//g;s/ //g'
</code></pre>
<h3>代码解释</h3>
<p>​<code>https://api.github.com/repos/lhc70000/iina/releases/latest</code>​这里用的是 GitHub 的官方 API，格式为<code>https://api.github.com/repos/{项目名}/releases/latest</code>​<br />
打开上述链接后，可见包含下述字段的内容：</p>
<pre><code class="language-bash">&quot;html_url&quot;: &quot;https://github.com/lhc70000/iina/releases/tag/v0.0.15.1&quot;,
&quot;id&quot;: 10774475,
&quot;node_id&quot;: &quot;MDc6UmVsZWFzZTEwNzc0NDc1&quot;,
&quot;tag_name&quot;: &quot;v0.0.15.1&quot;,
&quot;target_commitish&quot;: &quot;0.0.15.1&quot;,
&quot;name&quot;: &quot;v0.0.15.1&quot;,
</code></pre>
<p>那么这里的<code>tag_name</code>​就是我们所需要的东西啦</p>
<h4>weget参数</h4>
<p>​<code>wget -qO- -t1 -T2</code>​，在这里，我们使用了 4 个参数，分别是<code>q,O-,t1,T2</code>​</p>
<ol>
<li>​<code>-q</code>​: q 就是 quiet 的意思了，没有该参数将会显示从请求到输出全过程的所有内容，这肯定不是我们想要的。</li>
<li>​<code>-O-</code>​: <code>-O</code>​是指把文档写入文件中，而<code>-O-</code>​是将内容写入标准输出，而不保存为文件。（注：这里是大写英文字母 O (Out)，不是数字 0）</li>
<li>​<code>-t1,-T2</code>​: 前者是设定最大尝试链接次数为 1 次，后者是设定响应超时的秒数为 2 秒，两者可以防止失败后反复获取，导致后续脚本无法执行。</li>
</ol>
<h4>筛选参数</h4>
<ol>
<li>​<code>grep &quot;tag_name&quot;</code>​: grep 是 Linux 一个强大的文本搜索工具，在本代码中输出 tag_name 所在行，即输出<code>&quot;tag_name&quot;: &quot;v0.0.15.1&quot;,</code>​</li>
<li>​<code>head -n 1</code>​: <code>head -n</code>​用于显示输出的行数，考虑到某些项目可能存在多个不同版本的 tag_name，这里我们只要第一个。</li>
<li>​<code>awk -F &quot;:&quot; '{print $2}'</code>​: awk 主要用于文本分析，在这里指定<code>:</code>​为分隔符，将该行切分成多列，并输出第二列。于是我们得到了<code>(空格)&quot;v0.0.15.1&quot;,</code>​</li>
<li>​<code>sed 's/\&quot;//g;s/,//g;s/ //g'</code>​: 在这里 sed 用于数据查找替换，如<code>sed 's/要被取代的字串/新的字串/g'</code>​ ，因此本段命令可分为 3 个，以分号分隔。<code>s/\&quot;//g</code>​即将引号删除（反斜杠是为了防止引号被转义），以此类推，最终留下我们需要的内容：<code>v0.0.15.1</code>​。</li>
</ol>
<p>‍</p>
