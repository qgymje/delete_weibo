# 删除当前页面的微博

打开console，把代码粘帖进去，平均半秒删除一条

'''js
'use strict';
 
var s = document.createElement('script');
s.setAttribute(
  'src',
  'https://lib.sinaapp.com/js/jquery/2.0.3/jquery-2.0.3.min.js'
);

function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function delete_action(callback) {
    var deletes_btns = $('a[action-type="feed_list_delete"]')
    var count = deletes_btns.length
    var callback_is_called = false
    if(count > 0){
        for(var i=0;i<count;i++){
            deletes_btns[i].click()
            $('a[action-type="ok"]')[0].click()
            await sleep(500)
            if (count - i < 10) {
                if (!callback_is_called) {
                    callback()
                    callback_is_called = true
                }
            }
        }

        setTimeout(function(){
            delete_action(callback)
        }, 100)
    }
}

function scroll_to_bottom() {
    $('html, body').animate({ scrollTop: $(document).height() }, 'slow');
}

s.onload = function() {
    delete_action(scroll_to_bottom)    
};
document.head.appendChild(s);
'''
