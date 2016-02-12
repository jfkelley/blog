---
title: 'Dota 2 Ability Draft Statistical Analysis - Part 1'
author: joefkelley
layout: post
date: 2015-01-22
url: /765
sfw_pwd:
  - 7xXFm0wpuyas
categories:
  - Dota 2
  - Math

---
Dota 2's ability draft game mode is sorely lacking some statistical analysis. The awesome dota-metrics blog did a good post about it a while back (<https://dotametrics.wordpress.com/2014/06/11/random-ability-draft-hero-win-rates/>) but only talked about which heroes were best, which misses the real meat of ability draft: the abilities!

I downloaded ability draft data from the Dota 2 web api for a period between Nov 9-12 2014, about 220,000 matches. Note that this is after the Phantom Lancer and Bloodseeker reworks, but before Oracle was added.

If you just want the data, it is available here: <https://www.google.com/fusiontables/DataSource?docid=1ig78pmXSQJywdi7Rgj9ttONDJd-eeQTyHeEhB6Oe>.

## The Top 10

The most obvious question and most obviously useful data I can provide is which abilities have the highest win rates. Here are the top 10:

#### 1.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/necrolyte_heartstopper_aura_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Necrolyte Hearstopper Aura - 61.17%

95% confidence interval: 60.63% - 61.71%

The big number one winner was a shocker to me. I almost never picked this ability, but the simple fact is: it wins games. Maybe it being underestimated is precisely why it is so successful. I'm still frankly stumped by why this skill does so well, but the numbers are clear. Maybe because it does give a small and present boost to all phases of the game: it's annoying to lane against, and has the potential to do a very large amount of damage in drawn-out team fights.

#### 2.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/alchemist_chemical_rage_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Alchemist Chemical Rage - 60.05%

95% confidence interval: 59.51% - 60.59%

This is what I expected to be number one. It's the best steroid for a carry (or any hero) in the game. Alchemist has pretty bad stats, but makes up for it with this ultimate. But put the ultimate on someone with better stats and you've got a recipe for success. It also is relatively difficult to deal with in a disorganized pub game. The best way to kill someone with chemical rage is to have everyone on your team focus the one target all at the same time, which can be difficult to coordinate.

#### 3.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/viper_corrosive_skin_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Viper Corrosive Skin - 59.61%

95% confidence interval: 59.07% - 60.15%

This was another surprise for me. It's a very strong passive, no doubt, but number four is extremely high. But it makes you very hard to lane against, especially in 1v1 lanes, and gives you a surprising amount of resilience in the mid game. This is the main reason Viper is known as a tanky mid-game carry. 25% magic resistance is no joke, and the 25% move speed slow helps equally well running from any hero.

#### 4.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/warlock_rain_of_chaos_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Warlock Chaotic Offering - 59.26%

95% confidence interval: 58.69% - 59.83%

Chaotic Offering being so high tells me two things: big team fights are important and ultimates that are hard to screw up are good. Chaotic Offering has a huge aoe; it's pretty impossible to miss. It's a solid stun, pierces magic immunity, does lots of damage... it's an all-around great spell. Warlock's other spells are kind of lackluster, but it's pretty hard to lose a game if you're able to farm aghs and refresher and drop 4 golems in a 5v5 fight. Also, nobody knows to buy diffusal blade against this ultimate.

#### 5.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Leshrac Pulse Nova - 58.73%

95% confidence interval: 58.15% - 59.34%

Pulse nova is a very strong team fight spell, weakened in other modes by Leshrac's poor survivability. To be fully effective, it requires standing in the middle of a fight for a long period of time, which Leshrac has a hard time doing. But when paired with a hero with better stats or some other defensive abilities, it's easy to shore up that weakness. I also suspect that this spell is underestimated when playing against it, leading to similar success as heartstopper.

#### 6.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/abaddon_borrowed_time_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Abaddon Borrowed Time - 58.73%

95% confidence interval: 58.18% - 59.27%

Another strong defensive passive. There seems to be a pattern emerging here - simply not dying is a good way to win, and abaddon's ult is one of the best ways to stay alive.

#### 7.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_reincarnation_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Wraith King Reincarnation - 58.66%

95% confidence interval: 58.11% - 59.20%

Again, not dying seems pretty strong. The recently-added slow also makes this spell much more effective in team fights.

#### 8.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sandking_burrowstrike_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Sand King Burrowstrike - 58.26%

95% confidence interval: 57.71% - 58.80%

This spell is good in many different ways. It's an efficient gap-closer for initiation, it's a good escape mechanism, and it is an aoe stun and nuke (sort of). All of those things are extremely useful. This skill is sand king's sole reason for being an effective early-game hero. The skill is versatile enough to fit into almost any AD build.

#### 9.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/riki_permanent_invisibility_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Riki Permanent Invisibility - 57.82%

95% confidence interval: 57.25% - 58.36%

Riki's defining spell. Invisibility gets a lot of flack for being only good because poor players don't buy dust/gems/wards, but I think that's underestimating how difficult it is in practice to counter this ability. Being able to stay invisible while casting spells is devastating when paired with something stronger than smoke screen. As an example, when paired with Slardar's slithereen crush, the combo has a 69.4% win rate. And there are many ways to make the skill equally powerful and hard to shut out. It being moved to a non-ultimate opens up some more powerful combinations (e.g. Pulse Nova, Ravage, Duel) and just makes it that much stronger.

#### 10.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sven_storm_bolt_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Sven Storm Bolt - 57.54%

95% confidence interval: 56.99% - 58.08%

Not flashy, but this spell is a workhorse. Never underestimate the usefulness of a simple reliable disable. It's a true stun, doesn't require getting close, and has a pretty short cooldown. A great bread-and-butter ability.

## The Bottom 10

#### 1.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/furion_force_of_nature_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Nature's Prophet Nature's Call - 42.83%

95% confidence interval: 42.07% - 43.59%

I think people overestimate how much this ability contributes to Nature's Prophet's viability. He's often more effectively played as a highly-mobile ganker than as an early pusher. Early pushes require a specific team composition and coordination, and too often summoning treants = feeding treants.

#### 2.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/obsidian_destroyer_astral_imprisonment_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Obsidian Destroyer Astral Imprisonment - 43.16%

95% confidence interval: 42.34% - 43.99%

It's simply too easy to screw up with this spell. It's hard to use effectively either as an initiation or as a saving tool, and it's usefulness at stealing int is minimal; it's only good on OD because he has two other spells that synergize with it.

#### 3.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tinker_rearm_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Tinker Rearm - 43.75%

95% confidence interval: 43.12% - 44.38%

I used to be a strong believer in this spell being undervalued in AD, but it appears I was probably wrong. There are two reasons for this: there aren't that many spells that are worth comboing with rearm, and many heroes don't have nearly enough mana to sustain its use. It usually requires you to get BoT, Soul Ring, and one other mana item before you can be effective.

#### 4.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/lycan_summon_wolves_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Lycan Summon Wolves - 44.25%

95% confidence interval: 43.40% - 45.10%

When Lycan summons wolves in other modes, all three of his other spells buff them up. When they're on their own, they're pretty weak.

#### 5.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/pugna_decrepify_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Pugna Decrepify - 44.52%

95% confidence interval: 43.81% - 45.23%

Just like Astral Imprisonment, it's too easy to make mistakes with Decrepify. It also requires that you draft pretty much purely magical damage, so it greatly restricts your options.

#### 6.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_bloodrage_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Bloodseeker Bloodrage - 45.06%

95% confidence interval: 44.50% - 45.61%

Yet again, too easy to screw up. This spell is especially interesting because it's perfectly symmetric. It requires a lot of situational awareness to use properly.

#### 7.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sniper_assassinate_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Sniper Assassinate - 45.06%

95% confidence interval: 44.51% - 45.62%

It's admittedly satisfying to get kills with this spell, but you really want more out of an ultimate. If you're nuking, you either want higher damage (Laguna Blade, Finger of Death) or something that affects more targets (Epicenter, Ravage).

#### 8.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tinker_march_of_the_machines_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Tinker March of the Machines - 45.14%

95% confidence interval: 44.42% - 45.86%

A decent farming tool, but with a huge cooldown and huge mana cost. You pretty much have to have rearm with this for it to be useful.

#### 9.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/ancient_apparition_chilling_touch_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Ancient Apparition Chilling Touch - 45.22%

95% confidence interval: 44.32% - 46.13%

There's definitely a pattern here. Any ability with the potential to hurt your team is bad. Chilling touch also is by far most useful in the first few levels of the game, but it requires some serious coordination to make it work at that point.

#### 10.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bane_enfeeble_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Bane Enfeeble - 45.32%

95% confidence interval: 44.14% - 46.50%

Honestly this is probably just a weak spell in general. Its long duration helps with laning, but later in the game, you'd probably rather have a true disable.

## Overrated

[<img src="/assets/wr_vs_pr.png" alt="wr_vs_pr" class="alignnone size-full wp-image-784" />][1]

Above is a simple linear fit of the abilities' win rates vs their pick frequency. Here are the abilities that underperformed based on how often they are picked - i.e., the ones furthest below the fit line.

#### 1.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sniper_assassinate_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Sniper Assassinate

Picked 13.59%, Won 45.06%, Under expectation by 5.81%

#### 2.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bloodseeker_bloodrage_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Bloodseeker Bloodrage

Picked 13.48%, Won 45.06%, Under expectation by 5.76%

#### 3.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/tinker_rearm_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Tinker Rearm

Picked 10.47%, Won 43.75%, Under expectation by 5.75%

#### 4.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/earthshaker_enchant_totem_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Earthshaker Enchant Totem

Picked 13.92%, Won 45.53%, Under expectation by 5.48%

#### 5.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/axe_culling_blade_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Axe Culling Blade

Picked 13.22%, Won 45.46%, Under expectation by 5.25%

#### 6.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_assassin_coup_de_grace_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Phantom Assassin Coup de Grace

Picked 13.85%, Won 45.75%, Under expectation by 5.23%

#### 7.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/furion_force_of_nature_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Nature's Prophet Nature's Call

Picked 7.19%, Won 42.83%, Under expectation by 5.23%

#### 8.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/razor_static_link_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Razor Static Link

Picked 13.45%, Won 45.83%, Under expectation by 4.98%

#### 9.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/phantom_lancer_phantom_edge_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Phantom Lancer Phantom Rush

Picked 12.94%, Won 45.70%, Under expectation by 4.88%

#### 10.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/skeleton_king_mortal_strike_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Wraith King Mortal Strike

Picked 14.19%, Won 46.31%, Under expectation by 4.82%

## Underrated

Similarly, we can also look at the skills that are underrated - farthest above the line. The top abilities by this metric are almost all just the same as the top 10 winrate abilities since there are so many abilities that are almost always picked when they are available. What is more useful is those that are picked a little less often, yet still very underrated. These are the ones that make a very good second pick, third, or even fourth pick. Here are the top 10 abilities over expectation, with a pick rate under 13%:

#### 1.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/leshrac_pulse_nova_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Leshrac Pulse Nova

Picked 11.53%, Won 58.75%, Over expectation by 8.78%

#### 2.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/warlock_rain_of_chaos_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Warlock Chaotic Offering

Picked 12.85%, Won 59.26%, Over expectation by 8.72%

#### 3.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/juggernaut_healing_ward_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Juggernaut Healing Ward

Picked 6.87%, Won 53.83%, Over expectation by 5.91%

#### 4.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/pugna_nether_ward_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Pugna Nether Ward

Picked 10.85%, Won 55.41%, Over expectation by 5.75%

#### 5.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/enigma_midnight_pulse_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Enigma Midnight Pulse

Picked 7.96%, Won 53.96%, Over expectation by 5.56%

#### 6.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/sniper_shrapnel_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Sniper Shrapnel

Picked 7.36%, Won 53.05%, Over expectation by 4.92%

#### 7.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/kunkka_ghostship_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Kunkka Ghostship

Picked 11.51%, Won 54.30%, Over expectation by 4.35%

#### 8.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/disruptor_static_storm_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Disruptor Static Storm

Picked 10.30%, Won 53.71%, Over expectation by 4.29%

#### 9.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/witch_doctor_voodoo_restoration_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Witch Doctor Voodoo Restoration

Picked 6.89%, Won 52.15%, Over expectation by 4.22%

#### 10.  <img src="http://cdn.dota2.com/apps/dota2/images/abilities/bounty_hunter_track_hp2.png" width="30" height="30" style="vertical-align:bottom;margin:5px" />Bounty Hunter Track

Picked 12.74%, Won 54.59%, Over expectation by 4.10%

## Other Observations

#### Single-attack buffs

One of the "dream builds" in Ability Draft is the one-shot right-clicker. Some combination of Tidebringer, Walrus Punch, Shadow Walk, Enchant Totem, Vendetta, or any crit. But these are all fairly high-priority picks, so it's hard to get two of them, let alone three. Out of all of those abilities, only Tidebringer has an above-50 win rate, at an admittedly high 53.57%. The rest of them are below 50%, with Coup de Grace at a shockingly-bad 45.75%, putting it in the bottom 25. Unless you've got back-to-back picks or close to it, I think it's too risky to go for this kind of build.

#### Becoming ranged

Another highly-prioritized idea is turning melee heros into ranged ones by drafting either an orb attack (poison attack, frost arrows, glaives of wisdom, or arcane orb) or Sniper's Take Aim. Arcane Orb is bad because it's so mana intensive: win rate of x%. The rest all above 49% and below 51%. This is pretty lackluster and probably not worthy of a first-pick. However, this could be skewed by ranged heroes picking these skills; maybe they're more effective on melee.

#### Stuns are good, Slows are bad

There are a lot of stuns in the top half, and a lot of slows in the bottom half. Examples:

Good stuns: Storm Bolt (57.54%), Aftershock (56.09%), Slithereen Crush (54.85%), Paralyzing Cask (54.59%), Impale (53.90%)

Bad slows: Concussive Shot (49.59%), Frozen Sigil (49.40%), Poison Touch (47.59%), Viscous Nasal Goo (45.59%), Open Wounds (45.55%)

## Coming up...

In the next post, I'm planning on focusing on pairs of abilities - abilities that work well together - but it might be delayed a bit as I wait for more data to be collected to make the analysis statistically significant. But I'm also open to suggestions. What kinds of data would you like to see? Leave a comment or email me.

 [1]: /assets/wr_vs_pr.png
