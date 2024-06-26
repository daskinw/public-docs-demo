---
description: Hint content block
---

# Hint

Hints are a great way to bring the reader’s attention to specific elements in your documentation.

There are 4 different types of hints, and both [inline content](../editor/inline.md) and [formatting](../editor/formatting.md) are supported.

### Example of a hint

{% hint style="info" %}
**Info hints** are great for showing general information, or providing tips and tricks.
{% endhint %}

{% hint style="success" %}
**Success hints** are good for showing positive actions or achievements.
{% endhint %}

{% hint style="warning" %}
**Warning hints** are good for showing important information or non-critical warnings.
{% endhint %}

{% hint style="danger" %}
**Danger hints** are good for highlighting destructive actions or raising attention to critical information.
{% endhint %}

{% hint style="info" %}
**This is a heading**

This is a line

This is an inline <img src="../../.gitbook/assets/notification.png" alt="" data-size="line"> image

This is a second <mark style="color:orange;background-color:purple;">line</mark>
{% endhint %}

### Representation in Markdown

```markdown
{% raw %}
{% hint style="info" %}
**Info hints** are great for showing general information, or providing tips and tricks.
{% endhint %}

{% hint style="success" %}
**Success hints** are good for showing positive actions or achievements.
{% endhint %}

{% hint style="warning" %}
**Warning hints** are good for showing important information or non-critical warnings.
{% endhint %}

{% hint style="danger" %}
**Danger hints** are good for highlighting destructive actions or raising attention to critical information.
{% endhint %}

{% hint style="info" %}

### This is a heading

This is a line

This is an inline <img src=".gitbook/assets/notification.png" alt="" data-size="line"> image

This is a second <mark style="color:orange;background-color:purple;">line</mark>
{% endhint %}
{% endraw %}
```
