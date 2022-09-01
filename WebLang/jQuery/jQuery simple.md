```js
$(document).mouseup(function(e){
 var _con = $('目标区域 '); 
 if(!_con.is(e.target) && _con.has(e.target).length === 0){   
     // do something 
  }
});
```

```js
if($date){
  $checksql="SELECT wedding_time FROM `#@__orders` WHERE compereid='".$compereid."' AND date='".$date."'";
  $dsql->SetQuery($checksql);
  $dsql->Execute();
  $res_arr=Array();
  while($res=$dsql->GetArray()){
    $res_arr[]=$res['wedding_time'];
  }
  $jsonString=json_encode($res_arr);
  echo $jsonString;
}
```

```js
$(".bootstrapDatepickr-day").on("click",function(){
  setTimeout(function(){
   var check_date={
     date:$("#calendar1").val(),
     compereid:$("input[name='compereid']").val(),
   };
   $.ajax({
     cache: false,
     type: "POST",
     url: "ajax_check.php",
     data: check_date,
     async: false,
     error: function(request) {
      alert("发送请求失败！");
     },
     success: function(data) {
      var myjson="";
      eval('myjson=' + data + ';');
      for (var i = 0; i < myjson.length; i++) {
        $("#check_time option").each(function(a,n){
         if($(n).text()==myjson[i]){
           $(n).attr("disabled",true);
         };
        });
      }
     }
   });
  },50);
});
```

```js
document.onkeydown=keyDownSearch;
function keyDownSearch(e) {
  // 兼容FF和IE和Opera
  var theEvent = e || window.event;
  var code = theEvent.keyCode || theEvent.which || theEvent.charCode;
  if (code == 13) {
   var disp=$("#dialog-login").css("display");
   if(disp=="block"){
     login();
     return false;
   }
  }
  return true;
}
```

