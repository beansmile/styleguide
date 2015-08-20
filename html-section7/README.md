# 模板中的HTML

#### [建议] 模板代码的缩进优先保证 `HTML` 代码的缩进规则。

示例：

```html
<!-- good -->
{if $display == true}
<div>
  <ul>
  {foreach $item_list as $item}
    <li>{$item.name}<li>
  {/foreach}
  </ul>
</div>
{/if}

<!-- bad -->
{if $display == true}
  <div>
    <ul>
  {foreach $item_list as $item}
    <li>{$item.name}<li>
  {/foreach}
    </ul>
  </div>
{/if}
```
