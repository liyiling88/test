# localStorage+service缓存+sessionStorage的区别
<table width="100%" style="table-layout:fixed">
   <tr>
      <td style="100px">区别</td>
      <td style="width:25%">localStorage  </td>
      <td>sessionStorage  </td>
      <td>Service服务</td>
   </tr>
   <tr>
      <td>生命周期</td>
      <td style="width:25%">存储的数据被永久保存在本地，除非被清除</td>
      <td>生命周期为当前窗口或标签页，关闭标签页或浏览器后被清除（标签页指浏览器顶层的标签）</td>
      <td>应用整个生命周期结束的时候（关闭浏览器）才会被清除</td>
   </tr>
   <tr>
      <td>作用</td> 
      <td style="width:25%">存储客户端临时信息的对象；</td>
      <td>存储客户端临时信息的对象；</td>
      <td>主要用于封装一个http的通信服务以便于不同的controller在
         不同场景中重复调用，提供共享的数据、方法
      </td>
   </tr>
   <tr>
      <td>特点</td>
      <td style="width:25%">
         <p>1、只能存储字符串类型的对象；&nbsp;</p>
         <p>2、存储的数据需要用户手动才能清除；</p>
         <p>3、相同浏览器的不同页面间可以共享其存储的数据。</p>
      </td>
      <td>
         <p>1、只能存储字符串类型的对象；</p>
         <p>2、窗口或标签页被永久关闭了，存储的数据也就被清空了；</p>
         <p>3、相同浏览器不同页面间无法共享其存储的信息。</p>
      </td>
      <td>
         <p>1、多个页面或多个控制器之间共享数据（包括请求数据库的数据、模拟的数据和用户输入的数据）；</p>
         <p> 2、不会频繁的发生变化数据，无需重复向后台进行同样的请求获取同样的数据，而用缓存来节省时间和带宽。</p>
      </td>
   </tr>
   <tr>
      <td>应用场景</td>
      <td style="width:25%">
        <p>1、保存用户配置项：</p>
        <p>（1）记住用户名，登录密码（用于长期登录）；</p>
        <p>（2）判断用户是否已登录。</p>
        <p>2、保存用户的购物车信息。</p>
        <p>3、离线操作：离线可以保存，有网络直接提交。</p>
        <p>4、存储该浏览器对该页面的访问次数。</p>
      </td>
      <td>
        <p>1、敏感账号一次性登录（超过时间就必须重新登录）。</p>
        <p>2、遇到一些内容特别多的表单，把表单页面拆分成多个子页面，然后按步骤引导用户填写。</p>
        <p>3、进行页面之间的传值：例如，从A页面获取的数据，需要在B页面发送给后端，需要sessionStorage将数据从A页面传递到B页面。</p>
      </td>
      <td>
        <p>1、定义共享的模拟数据。</p>
        <p>2、共享获取到的后台数据，对后台数据进行缓存，以便各个页面都可以获取数据，节约请求后台资源，
           但是当后台数据在页面上被用户修改时，需要重新从后台获取，而不能直接从service里直接获取。</p>
      </td>
   </tr>
   <tr>
      <td rowspan="2">共同点</td>
      <td colspan="2" style="width:50%">
        <p>1、不同浏览器无法共享localStorage或sessionStorage中的信息。</p>
        <p>2、仅在客户端（即浏览器）中保存，不参与和服务器的通信。</p>
        <p>3、保存的数据不会再发送给服务器，避免带宽浪费。</p>
      </td>
      <td></td>
   </tr>
   <tr>
      <td colspan="3">
        <p>1、存储数据，但不能存储过多的数据。</p>
        <p>2、都可以进行多个页面或控制器之间的传值。</p>
      </td>
   </tr>
   <tr>
      <td>方法</td>
      <td colspan="2" style="width:80%">
        <p>getItem(key): 获取键值key对应的值</p>
        <p>setItem(key, value): 添加数据，键值为key，值为value</p>
        <p>removeItem(key): 移除键值为key的数据</p>
        <p>clear():清除所有数据</p>
      </td>
      <td>
<pre>
function getTest(isReload) { //用户传入状态
 var deferred = $q.defer();
  if (testList.length==0 || isReload) { 
//当初始状态和isRload状态为true时，向后台请求数据
    $http({
     method:'get',
     url:"data/askdata.json",
    }).success(function(resp){
     resp.push(data);
     deferred.resolve(resp);
    });
   } else {
//当isReload状态为undefined或false时，
//返回缓存中的数据
     deferred.resolve(testList);
   }
  return deferred.promise;
};
</pre>
      </td>
   </tr>
</table>
