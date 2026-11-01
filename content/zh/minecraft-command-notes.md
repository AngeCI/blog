---
date: "2026-10-31T19:34:20+00:00"
type: "post"
title: "Minecraft 指令及伺服器管理筆記"
description: ""
categories:
  - "Minecraft"
  - "Notes 筆記"
---

> 本頁面目前正在**施工中**，敬請留意本頁面的**持續更新**！

```
/effect give @p instant_health 999999 15 true
```

```
/give @p minecraft:villager_spawn_egg{EntityTag:{NoAI:1b,PersistenceRequired:1b,VillagerData:{type:"minecraft:plains",profession:"minecraft:librarian",lvl:5},Brain:{memories:{job_site:{pos:[I;0,0,0],dimension:"minecraft:overworld"}}},Offers:{Recipes:[{rewardExp:true,maxUses:114514,uses:0,max:null,buy:{id:"minecraft:emerald",Count:12b},sell:{id:"minecraft:enchanted_book",Count:1b,tag:{Enchantments:[{id:"minecraft:mending",lvl:1}]}}}]},id:"minecraft:villager"}} 64
```

```
/summon villager ~ ~1 ~ {VillagerData:{profession:farmer,level:2,type:swamp},NoAI:1,ActiveEffects:[{Id:10,Amplifier:0,Duration:999999}],Offers:{Recipes:[{buy:{id:emerald,Count:1},sell:{id:enchanted_book,Count:1,tag:{StoredEnchantments:[{id:mending,lvl:1}]}},rewardExp:0b,maxUses:9999999}]}}
```

```
/give @p bow{Unbreakable:1b,Enchantments:[{id:"minecraft:infinity",lvl:1s}]} 1
```

```
/execute positioned as @e run summon minecraft:lightning_bolt
```

```
/scoreboard objectives add Deaths deathCount
/scoreboard objectives setdisplay sidebar Deaths
```