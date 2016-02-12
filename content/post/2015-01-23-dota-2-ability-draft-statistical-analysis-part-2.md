---
title: 'Dota 2 Ability Draft Statistical Analysis - Part 2'
author: joefkelley
layout: post
date: 2015-01-24
url: /788
sfw_pwd:
  - WpkJ5w3t9oUj
categories:
  - Dota 2
  - Math

---
In a [previous post][1], I started an analysis of Dota 2's Ability Draft mode. This is continued here, with a focus on increasingly fine-grained analysis. That post is probably a better place to start, if you haven't read it yet.

If you just want the data, I've uploaded it to a few Google fusion tables:

  * [Single Abilities][2]
  * [Abilities Melee vs Ranged][3]
  * [Pairs of Abilities][4]

## Ability Synergy

Probably the defining aspect of ability draft that makes it so fun is the ability to combine abilities from different heroes, and so I spent some time analyzing pairs of abilities.

One major disclaimer here is to take all of this with a grain of salt. This is based on a sample size of more than 300,000 games, but each pair of abilities is relatively rare, so one pair usually has a sample size of about 200-400. This makes the confidence in the accuracy of these statistics much lower than all of the previous ones. However, good abilities tend to be drafted much more often, so the strongest pairs have a higher sample size: tending towards 500-1,000. I feel it's enough of a sample size to still draw conclusions. In most cases, the "true" win rate is very likely to be within 1% of the measured rate. But I have used some discretion and totally omitted pairs that have a sample size below 50 or so, as those are not likely to be statistically significant, and instead are probably flukes. As an example, there is a single pair of abilities with a 100% win rate in the data: Riki Permanent Invisibility + Bounty Hunter Shadow Walk. Exactly one person played that pair once, and they managed to win. But you'd have to be braindead to think that's actually the strongest pair of abilities in AD.

Also because of the sample size problem, I did not even attempt to analyze triplets of abilities, or entire 4-ability builds. Specific 3- or 4- ability combinations are just too rare.

With that out of the way, let's look at the strongest combinations:

#### Top 20 ability pairs by win rate

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability 1
    </th>

    <th>
      Ability 2
    </th>

    <th>
      Win Rate
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      76.80%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_reincarnation_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Reincarnation
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Necrophos Heartstopper Aura
    </td>

    <td>
      75.04%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_berserkers_blood_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Berserker's Blood
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      74.80%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/riki_permanent_invisibility_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Riki Permanent Invisibility
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Leshrac Pulse Nova
    </td>

    <td>
      74.65%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_berserkers_blood_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Berserker's Blood
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/ursa_fury_swipes_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Ursa Fury Swipes
    </td>

    <td>
      74.53%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      74.18%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_reincarnation_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Reincarnation
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/viper_corrosive_skin_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Viper Corrosive Skin
    </td>

    <td>
      74.16%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Leshrac Pulse Nova
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/weaver_shukuchi_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Weaver Shukuchi
    </td>

    <td>
      73.47%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Aftershock
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/storm_spirit_ball_lightning_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Storm Spirit Ball Lightning
    </td>

    <td>
      73.34%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Necrophos Heartstopper Aura
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      73.19%
    </td>
  </tr>

  <tr>
    <td>
      11
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/weaver_shukuchi_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Weaver Shukuchi
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      73.05%
    </td>
  </tr>

  <tr>
    <td>
      12
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Necrophos Heartstopper Aura
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Leshrac Pulse Nova
    </td>

    <td>
      72.69%
    </td>
  </tr>

  <tr>
    <td>
      13
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/ursa_fury_swipes_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Ursa Fury Swipes
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      72.62%
    </td>
  </tr>

  <tr>
    <td>
      14
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Necrophos Heartstopper Aura
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/weaver_shukuchi_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Weaver Shukuchi
    </td>

    <td>
      72.21%
    </td>
  </tr>

  <tr>
    <td>
      15
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/warlock_fatal_bonds_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Warlock Fatal Bonds
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/batrider_sticky_napalm_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Batrider Sticky Napalm
    </td>

    <td>
      72.11%
    </td>
  </tr>

  <tr>
    <td>
      16
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Aftershock
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/obsidian_destroyer_arcane_orb_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Outworld Devourer Arcane Orb
    </td>

    <td>
      72.10%
    </td>
  </tr>

  <tr>
    <td>
      17
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_berserkers_blood_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Berserker's Blood
    </td>

    <td>
      71.81%
    </td>
  </tr>

  <tr>
    <td>
      18
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/lina_fiery_soul_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Lina Fiery Soul
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      71.73%
    </td>
  </tr>

  <tr>
    <td>
      19
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/viper_corrosive_skin_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Viper Corrosive Skin
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      71.70%
    </td>
  </tr>

  <tr>
    <td>
      20
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_reincarnation_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Reincarnation
    </td>

    <td>
      71.63%
    </td>
  </tr>
</table>

76.8% win rate! Think about that for a second, that's one hero in a game of 10 that, more often than not, totally takes over the game. Regardless of how your teammates are doing, you're still going to win more than 3/4 games. Of course, it's very rare to get two very good spells like that.

There are a few notable exceptions, but most of this list is just two good spells. One take-home point is this: picking two individually good abilities is usually better than two mediocre abilities that have a cool interaction. If you can get two of the real powerhouses, you're pretty much going to win, but that's unlikely in reality. Most games you're not going to get any of the top 20 or so abilities, let alone two.

What's more interesting is if I introduce the concept of synergy. That is, how much does the combination of two abilities contribute to the overall win rate, compared to each of them individually. To calculate this, I simply took the difference between the pair's win rate and the arithmetic mean of the individual abilities' win rates (arithmetic mean fit very slightly better than geometric mean).

#### Top 20 ability pairs by synergy

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability 1
    </th>

    <th>
      Ability 2
    </th>

    <th>
      Win Rate
    </th>

    <th>
      Synergy
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/warlock_fatal_bonds_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Warlock Fatal Bonds
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/batrider_sticky_napalm_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Batrider Sticky Napalm
    </td>

    <td>
      72.11%
    </td>

    <td>
      20.84%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Aftershock
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/obsidian_destroyer_arcane_orb_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Outworld Devourer Arcane Orb
    </td>

    <td>
      72.10%
    </td>

    <td>
      19.91%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_berserkers_blood_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Berserker's Blood
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/ursa_fury_swipes_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Ursa Fury Swipes
    </td>

    <td>
      74.53%
    </td>

    <td>
      19.82%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Aftershock
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/storm_spirit_ball_lightning_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Storm Spirit Ball Lightning
    </td>

    <td>
      73.34%
    </td>

    <td>
      19.32%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      76.80%
    </td>

    <td>
      18.23%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Aftershock
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/storm_spirit_static_remnant_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Storm Spirit Static Remnant
    </td>

    <td>
      69.89%
    </td>

    <td>
      17.64%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      68.38%
    </td>

    <td>
      17.55%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      74.18%
    </td>

    <td>
      17.50%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_berserkers_blood_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Berserker's Blood
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      74.80%
    </td>

    <td>
      16.89%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_berserkers_blood_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Berserker's Blood
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      71.58%
    </td>

    <td>
      16.63%
    </td>
  </tr>

  <tr>
    <td>
      11
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/riki_permanent_invisibility_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Riki Permanent Invisibility
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Leshrac Pulse Nova
    </td>

    <td>
      74.65%
    </td>

    <td>
      16.41%
    </td>
  </tr>

  <tr>
    <td>
      12
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/lina_fiery_soul_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Lina Fiery Soul
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bristleback_quill_spray_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bristleback Quill Spray
    </td>

    <td>
      66.97%
    </td>

    <td>
      16.40%
    </td>
  </tr>

  <tr>
    <td>
      13
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sniper_take_aim_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sniper Take Aim
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      70.81%
    </td>

    <td>
      16.09%
    </td>
  </tr>

  <tr>
    <td>
      14
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_echo_slam_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Echo Slam
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/obsidian_destroyer_arcane_orb_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Outworld Devourer Arcane Orb
    </td>

    <td>
      64.67%
    </td>

    <td>
      16.07%
    </td>
  </tr>

  <tr>
    <td>
      15
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/lina_fiery_soul_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Lina Fiery Soul
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      71.73%
    </td>

    <td>
      15.89%
    </td>
  </tr>

  <tr>
    <td>
      16
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/lich_frost_armor_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Lich Frost Armor
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/dazzle_shadow_wave_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Dazzle Shadow Wave
    </td>

    <td>
      66.35%
    </td>

    <td>
      15.87%
    </td>
  </tr>

  <tr>
    <td>
      17
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/slardar_slithereen_crush_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Slardar Slithereen Crush
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sniper_shrapnel_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sniper Shrapnel
    </td>

    <td>
      69.47%
    </td>

    <td>
      15.77%
    </td>
  </tr>

  <tr>
    <td>
      18
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/weaver_shukuchi_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Weaver Shukuchi
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      71.59%
    </td>

    <td>
      15.68%
    </td>
  </tr>

  <tr>
    <td>
      19
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/ursa_fury_swipes_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Ursa Fury Swipes
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      72.62%
    </td>

    <td>
      15.68%
    </td>
  </tr>

  <tr>
    <td>
      20
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Aftershock
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bristleback_quill_spray_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bristleback Quill Spray
    </td>

    <td>
      68.69%
    </td>

    <td>
      15.65%
    </td>
  </tr>
</table>

Now we're getting somewhere! Thirst + Chemical Rage is still in there, but there's a lot more interesting combinations here. Most of these are well-known to AD players: Aftershock combos well with a lot of things, two great carry passives are good together, Walrus Punch + Tidebringer, Permanent Invisibility + Pulse Nova. But there are some surprises too. Like Sniper Shrapnel + Slithereen Crush: I guess shrapnel lets you cut the distance to get in range for the crush.

One spell that is noticeably absent from both of these lists is Heartstopper Aura. I think this helps explain why it's at the top of the single abilities in the first place: it's good with everything. You don't really have to draft abilities that work with it, nor do you have to play it a certain way. It's always good. Here are the top skills to draft with it, and the pairs' win rates:

#### Top 10 Abilities with <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:4px" />Heartstopper Aura

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability
    </th>

    <th>
      Win Rate
    </th>

    <th>
      Synergy
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_reincarnation_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Reincarnation
    </td>

    <td>
      75.04%
    </td>

    <td>
      15.14%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      73.19%
    </td>

    <td>
      12.69%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Leshrac Pulse Nova
    </td>

    <td>
      72.69%
    </td>

    <td>
      12.82%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/weaver_shukuchi_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Weaver Shukuchi
    </td>

    <td>
      72.21%
    </td>

    <td>
      12.99%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/abaddon_borrowed_time_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Abaddon Borrowed Time
    </td>

    <td>
      71.50%
    </td>

    <td>
      11.66%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/warlock_rain_of_chaos_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Warlock Chaotic Offering
    </td>

    <td>
      71.10%
    </td>

    <td>
      11.02%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sandking_burrowstrike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sandking Burrowstrike
    </td>

    <td>
      70.63%
    </td>

    <td>
      11.08%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tidehunter_ravage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tidehunter Ravage
    </td>

    <td>
      70.49%
    </td>

    <td>
      12.53%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/riki_permanent_invisibility_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Riki Permanent Invisibility
    </td>

    <td>
      69.82%
    </td>

    <td>
      10.59%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      69.34%
    </td>

    <td>
      10.42%
    </td>
  </tr>
</table>

Those are all just good abilities in general; there's not that much actual game synergy. Contrast this with, say, the top abilities with Aftershock:

#### Top 10 Abilities with <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:4px" />Earthshaker Aftershock

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability
    </th>

    <th>
      Win Rate
    </th>

    <th>
      Synergy
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/storm_spirit_ball_lightning_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Storm Spirit Ball Lightning
    </td>

    <td>
      73.34%
    </td>

    <td>
      19.32%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/obsidian_destroyer_arcane_orb_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Outworld Devourer Arcane Orb
    </td>

    <td>
      72.10%
    </td>

    <td>
      19.91%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_reincarnation_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Reincarnation
    </td>

    <td>
      71.08%
    </td>

    <td>
      13.36%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/storm_spirit_static_remnant_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Storm Spirit Static Remnant
    </td>

    <td>
      69.89%
    </td>

    <td>
      17.64%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/weaver_shukuchi_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Weaver Shukuchi
    </td>

    <td>
      68.75%
    </td>

    <td>
      11.70%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bristleback_quill_spray_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bristleback Quill Spray
    </td>

    <td>
      68.69%
    </td>

    <td>
      15.65%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/obsidian_destroyer_essence_aura_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Outworld Devourer Essence Aura
    </td>

    <td>
      68.39%
    </td>

    <td>
      15.22%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/abaddon_borrowed_time_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Abaddon Borrowed Time
    </td>

    <td>
      67.16%
    </td>

    <td>
      9.50%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sven_storm_bolt_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sven Storm Bolt
    </td>

    <td>
      66.49%
    </td>

    <td>
      9.52%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      66.06%
    </td>

    <td>
      7.73%
    </td>
  </tr>
</table>

There are a few that are just good (Chemical Rage, Borrowed Time), but the rest all have obviously strong interactions with aftershock, which aligns with what the synergy values show.

Another surprising absence is Sticky Napalm. It probably has the biggest reputation for being imbalanced in AD, but I don't think the numbers actually support that very well. Here are its top 10 pairs:

#### Top 10 Abilities with <img src="http://cdn.dota2.com/apps/dota2/images/abilities/batrider_sticky_napalm_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:4px" />Batrider Sticky Napalm

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability
    </th>

    <th>
      Win Rate
    </th>

    <th>
      Synergy
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/warlock_fatal_bonds_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Warlock Fatal Bonds
    </td>

    <td>
      72.11%
    </td>

    <td>
      20.84%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/warlock_rain_of_chaos_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Warlock Chaotic Offering
    </td>

    <td>
      69.84%
    </td>

    <td>
      14.62%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Alchemist Chemical Rage
    </td>

    <td>
      66.67%
    </td>

    <td>
      11.04%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/riki_permanent_invisibility_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Riki Permanent Invisibility
    </td>

    <td>
      65.80%
    </td>

    <td>
      11.44%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/dark_seer_ion_shell_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Dark Seer Ion Shell
    </td>

    <td>
      64.46%
    </td>

    <td>
      13.97%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/viper_corrosive_skin_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Viper Corrosive Skin
    </td>

    <td>
      64.46%
    </td>

    <td>
      9.15%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_reincarnation_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Reincarnation
    </td>

    <td>
      63.20%
    </td>

    <td>
      8.18%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_track_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Track
    </td>

    <td>
      61.94%
    </td>

    <td>
      8.94%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_aftershock_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Aftershock
    </td>

    <td>
      61.70%
    </td>

    <td>
      7.90%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/abaddon_borrowed_time_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Abaddon Borrowed Time
    </td>

    <td>
      61.52%
    </td>

    <td>
      6.56%
    </td>
  </tr>
</table>

Fatal Bonds was one I didn't expect, but it's easy to see why that would be so strong. Ion shell is also up there; it's widely regarded as an auto-win, and 64.46% is very good, but far from what I think most people would expect. Shadow Shaman Shackles is also highly talked-about, but that combo isn't even in the top 30, with a win rate of 56.53% and synergy of 5.89%.

Here are a few other top-10-with-X lists:

#### Top 10 Abilities with <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tinker_rearm_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:4px" />Tinker Rearm

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability
    </th>

    <th>
      Win Rate
    </th>

    <th>
      Synergy
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/undying_tombstone_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Undying Tombstone
    </td>

    <td>
      63.70%
    </td>

    <td>
      13.28%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Necrophos Heartstopper Aura
    </td>

    <td>
      56.60%
    </td>

    <td>
      4.18%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sandking_burrowstrike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sandking Burrowstrike
    </td>

    <td>
      53.88%
    </td>

    <td>
      2.75%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/treant_living_armor_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Treant Living Armor
    </td>

    <td>
      53.87%
    </td>

    <td>
      6.19%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/viper_corrosive_skin_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Viper Corrosive Skin
    </td>

    <td>
      53.85%
    </td>

    <td>
      2.09%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sven_storm_bolt_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sven Storm Bolt
    </td>

    <td>
      53.24%
    </td>

    <td>
      2.52%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/dragon_knight_dragon_blood_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Dragon Knight Dragon Blood
    </td>

    <td>
      52.94%
    </td>

    <td>
      5.33%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/undying_soul_rip_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Undying Soul Rip
    </td>

    <td>
      52.90%
    </td>

    <td>
      7.83%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/pugna_nether_ward_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Pugna Nether Ward
    </td>

    <td>
      52.78%
    </td>

    <td>
      3.08%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/riki_permanent_invisibility_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Riki Permanent Invisibility
    </td>

    <td>
      52.69%
    </td>

    <td>
      1.89%
    </td>
  </tr>
</table>

Basically, get Tombstone or don't bother. Living Armor and Nether Ward can also be good because they're relatively rarely picked, so you might be able to get one of them second, then rearm third. My thought process used to be that there are plenty of spells that work well with Rearm, but that's just not true. There are only 23 that bring it above a 50% win rate.

#### Top 10 Abilities with <img src="http://cdn.dota2.com/apps/dota2/images/abilities/riki_permanent_invisibility_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:4px" />Riki Permanent Invisibility

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability
    </th>

    <th>
      Win Rate
    </th>

    <th>
      Synergy
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Leshrac Pulse Nova
    </td>

    <td>
      74.65%
    </td>

    <td>
      16.41%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sandking_burrowstrike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sandking Burrowstrike
    </td>

    <td>
      71.10%
    </td>

    <td>
      13.17%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Necrophos Heartstopper Aura
    </td>

    <td>
      69.82%
    </td>

    <td>
      10.59%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sven_storm_bolt_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Sven Storm Bolt
    </td>

    <td>
      68.48%
    </td>

    <td>
      10.95%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_thirst_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bloodseeker Thirst
    </td>

    <td>
      68.42%
    </td>

    <td>
      11.12%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/abaddon_borrowed_time_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Abaddon Borrowed Time
    </td>

    <td>
      67.09%
    </td>

    <td>
      8.87%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tidehunter_ravage_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tidehunter Ravage
    </td>

    <td>
      66.79%
    </td>

    <td>
      10.44%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/slardar_slithereen_crush_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Slardar Slithereen Crush
    </td>

    <td>
      66.72%
    </td>

    <td>
      10.57%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/vengefulspirit_magic_missile_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Vengefulspirit Magic Missile
    </td>

    <td>
      65.85%
    </td>

    <td>
      10.90%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/batrider_sticky_napalm_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Batrider Sticky Napalm
    </td>

    <td>
      65.80%
    </td>

    <td>
      11.44%
    </td>
  </tr>
</table>

There are a lot of different types of abilities here. What this shows me is that there are multiple ways to play with Permanent Invisiblity. Pulse Nova is the clear winner, but there are some other really brutal combinations here. Initiation abilities (any of the stuns) are especially dangerous.

## Abilities for Melee vs Ranged

In the last post, I mentioned that ranged orb effects don't actually have a very high win rate, but I also theorized that this was possibly confounded by ranged heroes picking them. If you look at the top abilities by win rate for just melee heroes or just ranged heroes, they don't differ much from the global top abilities. What is more interesting, though, is to look at the abilities that have the greatest difference in win rate between the two categories.

#### Top difference in win rate (melee over ranged)

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability
    </th>

    <th>
      Melee Win Rate
    </th>

    <th>
      Ranged Win Rate
    </th>

    <th>
      Difference
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/axe_counter_helix_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Axe Counter Helix
    </td>

    <td>
      55.34%
    </td>

    <td>
      47.22%
    </td>

    <td>
      8.11%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/clinkz_searing_arrows_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Clinkz Searing Arrows
    </td>

    <td>
      55.09%
    </td>

    <td>
      47.07%
    </td>

    <td>
      8.02%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/viper_poison_attack_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Viper Poison Attack
    </td>

    <td>
      52.80%
    </td>

    <td>
      45.66%
    </td>

    <td>
      7.13%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/axe_berserkers_call_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Axe Beserker's Call
    </td>

    <td>
      54.77%
    </td>

    <td>
      48.48%
    </td>

    <td>
      6.29%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/drow_ranger_frost_arrows_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Drow Ranger Frost Arrows
    </td>

    <td>
      52.43%
    </td>

    <td>
      46.51%
    </td>

    <td>
      5.92%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_burning_spear_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Burning Spear
    </td>

    <td>
      50.94%
    </td>

    <td>
      45.55%
    </td>

    <td>
      5.39%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/legion_commander_duel_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Legion Commander Duel
    </td>

    <td>
      49.97%
    </td>

    <td>
      45.68%
    </td>

    <td>
      4.30%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/centaur_return_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Centaur Return
    </td>

    <td>
      52.76%
    </td>

    <td>
      48.79%
    </td>

    <td>
      3.97%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/centaur_double_edge_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Centaur Double Edge
    </td>

    <td>
      46.83%
    </td>

    <td>
      42.96%
    </td>

    <td>
      3.87%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/huskar_life_break_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Huskar Life Break
    </td>

    <td>
      49.51%
    </td>

    <td>
      45.92%
    </td>

    <td>
      3.59%
    </td>
  </tr>
</table>

As expected, the ranged orb attacks all made the top 10 greatest differences. Searing arrows seems especially strong on melee heroes, but the rest are still middle-of-the-pack. The other spells are all either ones that require you being up close anyway, or hurt yourself. The latter is probably better on melee strength heroes.

#### Top difference in win rate (ranged over melee)

<table class="data">
  <tr>
    <th>
      Rank
    </th>

    <th>
      Ability
    </th>

    <th>
      Melee Win Rate
    </th>

    <th>
      Ranged Win Rate
    </th>

    <th>
      Difference
    </th>
  </tr>

  <tr>
    <td>
      1
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/drow_ranger_marksmanship_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Drow Ranger Marksmanship
    </td>

    <td>
      47.56%
    </td>

    <td>
      55.24%
    </td>

    <td>
      7.68%
    </td>
  </tr>

  <tr>
    <td>
      2
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/drow_ranger_trueshot_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Drow Ranger Precision Aura
    </td>

    <td>
      46.06%
    </td>

    <td>
      53.69%
    </td>

    <td>
      7.62%
    </td>
  </tr>

  <tr>
    <td>
      3
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/medusa_mana_shield_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Medusa Mana Shield
    </td>

    <td>
      49.75%
    </td>

    <td>
      56.42%
    </td>

    <td>
      6.67%
    </td>
  </tr>

  <tr>
    <td>
      4
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/slark_essence_shift_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Slark Essence Shift
    </td>

    <td>
      47.69%
    </td>

    <td>
      52.82%
    </td>

    <td>
      5.13%
    </td>
  </tr>

  <tr>
    <td>
      5
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/luna_moon_glaive_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Luna Moon Glaive
    </td>

    <td>
      48.54%
    </td>

    <td>
      53.29%
    </td>

    <td>
      4.75%
    </td>
  </tr>

  <tr>
    <td>
      6
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/dragon_knight_elder_dragon_form_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Dragon Knight Elder Dragon Form
    </td>

    <td>
      47.05%
    </td>

    <td>
      51.70%
    </td>

    <td>
      4.66%
    </td>
  </tr>

  <tr>
    <td>
      7
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/broodmother_insatiable_hunger_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Broodmother Insatiable Hunger
    </td>

    <td>
      48.49%
    </td>

    <td>
      52.73%
    </td>

    <td>
      4.24%
    </td>
  </tr>

  <tr>
    <td>
      8
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/ursa_overpower_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Ursa Overpower
    </td>

    <td>
      44.95%
    </td>

    <td>
      48.78%
    </td>

    <td>
      3.85%
    </td>
  </tr>

  <tr>
    <td>
      9
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/life_stealer_feast_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Lifestealer Feast
    </td>

    <td>
      51.09%
    </td>

    <td>
      54.88%
    </td>

    <td>
      3.79%
    </td>
  </tr>

  <tr>
    <td>
      10
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      54.81%
    </td>

    <td>
      58.60%
    </td>

    <td>
      3.79%
    </td>
  </tr>
</table>

Drow ranger's skills are obvious, and Elder Dragon Form doesn't work on most melee heroes, so that's obvious too. I'm pretty surprised how strong mana shield is on ranged, though perhaps thats correlation and not causation, as it's good on int heroes, which are often ranged. The rest of the list are all auto-attack buffs that come from melee heroes.

Grow is a very often picked skill, and it seems that is entirely justified on ranged heroes. 58.60% is solid first-pick material.

Overpower, on the other hand, is also very often picked early, but that's probably a mistake! Yes, it is much better on ranged heroes, but it's still not breaking even. 48.78% is probably not worth a first pick.

## "The Dream"

Lastly, I want to address a subset of skills that I mentioned last time. The mythical "dream" build with the power to one-shot enemy heroes. I think the ultimate combination would probably be Tidebringer, Jinada, Enchant Totem, and Walrus Punch. But that specific combination has never been assembled in my 300k+ game sample. Three of the four has happened 8 times. But you can also substitute in a number of other spells: any crit, Grow, Shadow Walk, Vendetta, etc.

As I mentioned before, none of those abilities actually have a very good individual win rate:

<table class="data">
  <tr>
    <th>
      Ability
    </th>

    <th>
      Win Rate
    </th>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      56.37%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      53.38%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      48.77%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      48.53%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      48.29%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      47.75%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      47.03%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Critical Strike
    </td>

    <td>
      46.36%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      45.94%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      45.56%
    </td>
  </tr>
</table>

However, pairs of most of them certainly do much better:

<table class="data">
  <tr>
    <th>
      Ability 1
    </th>

    <th>
      Ability 2
    </th>

    <th>
      Win Rate
    </th>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      68.99%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      68.38%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      68.35%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      65.30%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      65.13%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      64.72%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      62.07%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      61.32%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      60.89%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      58.99%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      55.30%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      55.19%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      54.80%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      54.41%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      54.02%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tiny_grow_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tiny Grow
    </td>

    <td>
      53.69%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      53.08%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      52.12%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      52.08%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      52.01%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      51.98%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      51.87%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      51.76%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_tidebringer_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Kunkka Tidebringer
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      51.26%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      48.35%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      48.18%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      47.79%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      47.72%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Motral Strike
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/brewmaster_drunken_brawler_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Brewmaster Drunken Brawler
    </td>

    <td>
      47.42%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      47.41%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      46.44%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      46.18%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      45.89%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      45.50%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      45.28%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_blade_dance_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Juggernaut Blade Dance
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      44.85%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      44.64%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      44.60%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      44.30%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      44.07%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      44.04%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      43.79%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      43.75%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      43.70%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Earthshaker Enchant Totem
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      43.01%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tusk_walrus_punch_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Tusk Walrus Punch
    </td>

    <td>
      42.48%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_jinada_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Jinada
    </td>

    <td>
      41.37%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_wind_walk_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Bounty Hunter Wind Walk
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/nyx_assassin_vendetta_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Nyx Assassin Vendetta
    </td>

    <td>
      40.69%
    </td>
  </tr>

  <tr>
    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Wraith King Mortal Strike
    </td>

    <td>
      <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="24" height="24" style="vertical-align:bottom;margin:2px" />Phantom Assassin Coup De Grace
    </td>

    <td>
      40.23%
    </td>
  </tr>
</table>

Trying to distill all that down into a few simple ideas, I would say this:

  * Grow and Tidebringer are probably worth it on their own, especially on ranged.
  * If you don't have one of those two abilities, Enchant Totem + Walrus punch is the only other really strong option.
  * Unreliable crits are still bad, even with a second synergy skill, but Juggernaut's blade dance is least bad.

That's the last of the analysis I have planned, but if there's anything else you want to know, I would probably be happy to do some more. I might also do a post sharing and explaining some of the code I used to acquire, transform, and analyze the data.

 [1]: /765
 [2]: https://www.google.com/fusiontables/DataSource?docid=1ig78pmXSQJywdi7Rgj9ttONDJd-eeQTyHeEhB6Oe
 [3]: https://www.google.com/fusiontables/DataSource?docid=14v3fsZVAbVmkDuclYwtm1BDNki2P9k7S7Uc-ZWgf
 [4]: https://www.google.com/fusiontables/DataSource?docid=1h8MHneCVhgGw4hI04Hr6hxTkamhPbyCj8QGEWsP5
