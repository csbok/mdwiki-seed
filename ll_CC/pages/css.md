## hidden
화면상에서 엘리먼트를 숨길때 두가지 방식이 있다.
> &lt;div&gt;start&lt;/div&gt;
> &lt;div style="visibility:hidden"&gt;숨었다&lt;/div&gt;
> &lt;div&gt;end&lt;/div&gt;
이 방식은 자리 차지는 그대로 한 상태로 숨기만 하는 방식이라 한줄 공백이 나타나지만,
아래의 방식은 자리도 차지 않아서 end가 start 바로 다음 줄에 나타난다. 
> &lt;div&gt;start&lt;/div&gt;
> &lt;div style="display:none"&gt;위치도 차지 안한다&lt;/div&gt;
> &lt;div&gt;end&lt;/div&gt;
