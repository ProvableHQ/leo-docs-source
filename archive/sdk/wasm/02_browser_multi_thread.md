---
id: browser_multi_thread
title: Browser (Multi-Thread)
sidebar_label: Browser (Multi-Thread)
---
:::info
The multi-thread version of the Aleo Wasm package includes all classes in the **[single-thread](./01_nodejs.md)** version, plus a few more.
:::
## Classes

<dl>
<dt><a href="#wbg_rayon_PoolBuilder">wbg_rayon_PoolBuilder</a></dt>
<dd></dd>
</dl>

## Functions

<dl>
<dt><a href="#initThreadPool">initThreadPool(num_threads)</a> ⇒ <code>Promise.&lt;any&gt;</code></dt>
<dd></dd>
<dt><a href="#wbg_rayon_start_worker">wbg_rayon_start_worker(receiver)</a></dt>
<dd></dd>
</dl>

<a name="Address"></a>


## wbg\_rayon\_PoolBuilder
**Kind**: global class

* [wbg_rayon_PoolBuilder](#wbg_rayon_PoolBuilder)
    * [.numThreads()](#wbg_rayon_PoolBuilder+numThreads) ⇒ <code>number</code>
    * [.receiver()](#wbg_rayon_PoolBuilder+receiver) ⇒ <code>number</code>
    * [.build()](#wbg_rayon_PoolBuilder+build)

<a name="wbg_rayon_PoolBuilder+numThreads"></a>

### wbg_rayon_PoolBuilder.numThreads() ⇒ <code>number</code>
**Kind**: instance method of [<code>wbg\_rayon\_PoolBuilder</code>](#wbg_rayon_PoolBuilder)
<a name="wbg_rayon_PoolBuilder+receiver"></a>

### wbg_rayon_PoolBuilder.receiver() ⇒ <code>number</code>
**Kind**: instance method of [<code>wbg\_rayon\_PoolBuilder</code>](#wbg_rayon_PoolBuilder)
<a name="wbg_rayon_PoolBuilder+build"></a>

### wbg_rayon_PoolBuilder.build()
**Kind**: instance method of [<code>wbg\_rayon\_PoolBuilder</code>](#wbg_rayon_PoolBuilder)
<a name="initThreadPool"></a>

## initThreadPool(num_threads) ⇒ <code>Promise.&lt;any&gt;</code>
**Kind**: global function

| Param | Type |
| --- | --- |
| num_threads | <code>number</code> |

<a name="wbg_rayon_start_worker"></a>

## wbg\_rayon\_start\_worker(receiver)
**Kind**: global function

| Param | Type |
| --- | --- |
| receiver | <code>number</code> |
