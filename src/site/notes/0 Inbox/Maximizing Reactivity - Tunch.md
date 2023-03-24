---
{"dg-publish":true,"permalink":"/0-inbox/maximizing-reactivity-tunch/"}
---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Focused_developer_refactoring_complex_application_surrounded_by_code_fragments_floating_text_glowing_screen_cyberpunk_atmosphere_Blade_Runner-inspired_digital_painting_high_quality_Developer_hero_navigating_through_an_An-1.png" data-background-opacity="0.5" -->
# Reactive Programming
Mastering the Force

note:
tags:: #output/presentation [[3 Resources/Programming\|Programming]] [[4 Archive/Notes/Angular\|Angular]] [[4 Archive/Notes/Reactive Programming\|Reactive Programming]]

Brainstorm
- Advantages
	- Less code
	- Improved rendering performance
	- Clear side-effects
	- Less bugs
- Why?
	- Understanding
		- Basically functional programming: you know what goes in and what comes out
	- Stability
		- Immutability
		- But there's *more*: not just *what* is fixed as immutable, but also *when* is explicit
		- This means things change when they need to
		- Refactoring is easy, no need to consider the new thing everywhere
	- Performance
		- React rendering is shit because it doesn't know shit
		- Default Angular rendering is even worse
		- But if everything is reactive it knows perfectly
		- RxAngular: default zone-based renderer isn't very efficient, we need to change it out
- How?
	- State management: the best solution isn't to fix it, it's to skip it
	- Everything is a stream
	- The least amount of `Subject`s as possible  (they're fake)
	- No logic in event handler methods
	- Refactoring flow
		- Properties + modifying methods -> computed Observable
		- Event handler -> Subject trigger -> computed Observable
		- Sharing across components: BehaviorSubject-in-a-service
		- Hey wait this is state management
- What?
	- Everything `OnPush`
		- Pushes you to structure your app better, declaratively
		- Like strict mode
	- All your fields are `readonly` (except inputs)
	- Reactive-Angular library
		- \*rxLet
		- PushPipe
		- \*rxIf
			- else, suspense, error, complete
		- \*rxFor
	- Define observables from other observables in the constructor
		- The template handles subscribing, that's perfect
		- Reuse the same subscription in the template (rxLet)
	- Only `.subscribe()` if you *really* have no other option
	- Implement side effects with `tap()` and combine them in a `merge` so you only have a *single subscription in the entire component*!
	- Passing Observables in `Input()`s
	- Opting out of `Zone`
		- Run outside of Zone
		- Unpatch events
- Ummm forget everything I said
	- Signals
	- See SolidJS

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/distress.png" data-background-opacity="0.7" -->

#### Act 1
# Departure
Embrace the Reactive Way of the Force

---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_sharing_wisdom_with_other_developers_aboard_a_futuristic_spaceship_holographic_displays_showcasing_tips_and_best_practices_Angular_galaxy_in_the_background_Star_Wars_theme_discovering_reactive_programming_-1.png" -->

notes:

In a galaxy far, far away, on the distant planet of Codeonia, there lived a young developer named Dev Skywalker. Codeonia was a world filled with countless software projects, with developers working tirelessly to build and maintain applications.

One day, Dev Skywalker found themselves at the center of an epic quest. An ancient prophecy spoke of a hero who would bring balance to the world of software development, vanquishing the dark forces of bugs and performance issues, and ushering in an era of stability and performance.

---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Wise_hooded_mentor_teaching_Reactive_Programming_hologram_projection_technology-filled_room_detailed_Hokusai-inspired_style_dynamic_lighting_digital_illustration_high_definition_Developer_hero_navigating_through_an_Angul-1.png" -->

notes:

Dev's journey began when they stumbled upon a mysterious hologram. It contained a cryptic message from a legendary developer, Master Obi-Wan Codenobi, who introduced Dev to the ancient art of Reactive Programming. Intrigued by this newfound knowledge, Dev knew they had a mission to fulfill the prophecy.

---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/battle.png" -->

notes:

The path was treacherous, filled with confusing concepts and strange syntax. But Dev persevered, learning the ways of Reactive Programming and harnessing its power to build applications that were more efficient, less buggy, and easier to refactor.

Through countless hours of coding, Dev discovered the secrets of Reactive Programming, understanding the importance of immutability, observables, and managing state. Armed with this knowledge, Dev set out to face their greatest challenge yet: refactoring a sprawling, complex application plagued by bugs and performance issues.

The battle was fierce. Dev fought valiantly, using their newfound Reactive Programming skills to rewrite the code, untangling the web of state management and side-effects. As they worked, they saw how the reactive approach made the code more resilient to bugs and allowed them to refactor with confidence.

---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Victorious_developer_on_top_of_a_hill_binary_suns_triumphant_pose_futuristic_skyline_epic_scenery_Star_Wars_and_Moebius-inspired_digital_painting_high_resolution_Developer_hero_navigating_through_an_Angular_galaxy_jedi_d-0.png" -->

notes:

Finally, after days of intense effort, Dev emerged victorious. The application had been transformed into a shining example of stability and performance. News of their triumph spread across the galaxy, and Dev Skywalker became a symbol of hope for developers everywhere.

As Dev stood on a hill, watching the binary suns of Codeonia set, they knew their journey had just begun. There were still countless applications to be refactored and developers to be taught the ways of Reactive Programming. But Dev Skywalker was ready, prepared to face any challenge and bring balance to the world of software development.

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_looking_at_a_holographic_list_of_reactive_programming_benefits_floating_in_space_Angular_planets_in_the_background_a_futuristic_space_scene_with_a_Star_Wars_vibe_Developer_hero_navigating_through_an_Angula-3.png" data-background-opacity="0.5" -->

## Advantages of Reactive Programming

-   Improved rendering performance
-   Clear side-effects management
-   Easier refactoring
-   More maintainable and less buggy code

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_looking_at_a_holographic_list_of_reactive_programming_benefits_floating_in_space_Angular_planets_in_the_background_a_futuristic_space_scene_with_a_Star_Wars_vibe_Developer_hero_navigating_through_an_Angula-3.png" data-background-opacity="0.5" -->

## Stability & Immutability

-   Better control over state changes
-   Easier refactoring: no need to consider the new thing everywhere
-   Clearer dependencies between fields

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_looking_at_a_holographic_list_of_reactive_programming_benefits_floating_in_space_Angular_planets_in_the_background_a_futuristic_space_scene_with_a_Star_Wars_vibe_Developer_hero_navigating_through_an_Angula-3.png" data-background-opacity="0.5" -->

## Dependencies
Reactive programming makes dependencies between variables clear.

---

## Imperative dependencies

```ts
@Component({
  // ...
})
export class JediComponent implements OnInit {
  jediLevel: number;
  forceStatus: string;

  constructor(private jediService: JediService) {}

  ngOnInit() {
    this.jediService.getJediLevel().subscribe((level) => {
      this.jediLevel = level;
      this.updateForceStatus();
    });
  }

  updateForceStatus() {
    this.forceStatus = this.jediLevel >= 9000 ? 'Master' : 'Padawan';
  }
}
```

---

These are not the bugs you are looking for
![Image description](https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/these-are-not-the-droids.jpeg)

---
## Reactive dependencies

```ts
@Component({
  // ...
})
export class JediComponent {
  jediLevel$: Observable<number> = this.jediService.getJediLevel();
  forceStatus$: Observable<string> = this.jediLevel$.pipe(
    map((level) => level >= 9000 ? 'Master' : 'Padawan')
  );

  constructor(private jediService: JediService) {}
}
```

THIS IS THE WAY

---
<!-- .slide: data-auto-animate -->

## Refactor

```ts
@Component({
  selector: 'app-users',
  template: `
    <ul>
      <li *ngFor="let user of users">
        {{ user.name }} - {{ user.isOnline ? 'Online' : 'Offline' }}
      </li>
    </ul>
  `
})
export class UsersComponent implements OnInit {
  users: User[] = [];

  ngOnInit() {
    this.userService.getUsers().subscribe((users) => {
      this.users = users;
    });
  }
}
```

📣 I want online user count

---
<!-- .slide: data-auto-animate -->

## Refactor
### Imperative

```ts
@Component({
  selector: 'app-users',
  template: `
    <p>Online users: {{ onlineUsersCount }}</p>
    <ul>
      <li *ngFor="let user of users">
        {{ user.name }} - {{ user.isOnline ? 'Online' : 'Offline' }}
      </li>
    </ul>
  `
})
export class UsersComponent implements OnInit {
  users: User[] = [];
  onlineUsersCount = 0;

  ngOnInit() {
    this.userService.getUsers().subscribe((users) => {
      this.users = users;
      // This line could be easily missed, introducing a bug
      this.onlineUsersCount = users.filter((user) => user.isOnline).length;
    });
  }
}
```

---
<!-- .slide: data-auto-animate -->

## Refactor
### Reactive

User count is completely separated from users!

```ts
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-users',
  template: `
    <p>Online users: {{ onlineUsersCount$ | async }}</p>
    <ul>
      <li *ngFor="let user of users$ | async">
        {{ user.name }} - {{ user.isOnline ? 'Online' : 'Offline' }}
      </li>
    </ul>
  `
})
export class UsersComponent {
  users$ = this.userService.getUsers();
  onlineUsersCount$ = this.users$.pipe(
    map((users) => users.filter((user) => user.isOnline).length)
  );

  constructor(private userService: UserService) {}
}
```

---

![its over](https://i.imgflip.com/3kunzf.jpg)

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-5.png" data-background-opacity="0.7" -->

#### Act 2
# Initiation
Delving Deeper into RxAngular

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-7.png" data-background-opacity="0.7" -->

# The road of trials
Performance and state management

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-7.png" data-background-opacity="0.7" -->

The Road of Trials
## The Dark Side - Default Angular Rendering Performance

-   Default Angular rendering performance can be a drag
-   Even with OnPush change detection strategy, there's still room for improvement
-   Enter: Zone.js - the mysterious puppet master of Angular

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-7.png" data-background-opacity="0.7" -->

The Road of Trials
## The Function of Zone.js

-   Zone.js tracks and manages asynchronous tasks in Angular
-   It automatically triggers change detection "when needed"
-   But it comes with a cost: performance overhead

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-7.png" data-background-opacity="0.7" -->

The Road of Trials
## RxAngular Strikes Back
Boosting Performance

-   RxAngular helps optimize rendering performance by minimizing unnecessary change detection cycles
-   Use template components like *rxLet, *rxIf, and *rxFor for more efficient rendering

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-7.png" data-background-opacity="0.7" -->

The Road of Trials

\*rxLet only updates *the relevant part of* the template, instead of *marking all ancestors as dirty*.

```ts
<!-- Use *rxLet for more efficient rendering -->
<div *rxLet="data$; let data">
  {{ data.title }}: {{ data.description }}
</div>
```

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-7.png" data-background-opacity="0.7" -->

The Road of Trials
## Leaving the Zone

Unpatch events to avoid unnecessary change detection triggered by Zone.js

```html
<!-- Unpatching click event to avoid unnecessary change detection -->
<button (click.unpatch)="onClick()">Click me, I won't trigger change detection!</button>
```

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/yoda.png"  data-background-opacity="0.5" -->

The Road of Trials
## Change detection

> The best solution isn't to fix it, it's to skip it

---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-5.png" data-background-opacity="0.7" -->

# The Approach
Reactive Techniques

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/00000.png" data-background-opacity="0.5" -->

## Everything is a Stream

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-5.png" data-background-opacity="0.5" -->

Define everything through RxJS operators at construction time

```ts
@Component({
  selector: 'app-timer',
  template: `<p>{{ seconds$ | async }} seconds have passed.</p>`
})
export class TimerComponent implements OnDestroy {
  private onDestroy$ = new Subject<void>();
  seconds$ = interval(1000).pipe(takeUntil(this.onDestroy$));

  ngOnDestroy() {
    this.onDestroy$.next();
    this.onDestroy$.complete();
  }
}
```

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-5.png" data-background-opacity="0.5" -->

**Do not `subscribe()`.**

You want to *show* your stuff, right? Let the template subscribe.

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-5.png" data-background-opacity="0.5" -->

Class methods should be one line (`.next()`ing a Subject)

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-5.png" data-background-opacity="0.5" -->

Implement side effects with `tap()` and combine them in a `merge`

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_battling_against_non-reactive_code_epic_battle_fight_transforming_it_into_reactive_code_neon_lightsaber_illuminating_RxAngular_features_epic_Star_Wars-inspired_space_battle_scene_Developer_hero_navigating_-5.png" data-background-opacity="0.5" -->

```ts
@Component({ /* ... */ })
export class DeathStarComponent {
  private destruction$ = new Subject<void>();
  private shieldDeactivation$ = new Subject<void>();

  constructor() {
    const logDestruction = this.destruction$.pipe(
      tap(() => console.log('Death Star destroyed!'))
    );

    const logShieldDeactivation = this.shieldDeactivation$.pipe(
      tap(() => console.log('Death Star shield deactivated!'))
    );

    merge(logDestruction, logShieldDeactivation).subscribe();
  }

  destroy() {
    this.destruction$.next();
  }

  deactivateShield() {
    this.shieldDeactivation$.next();
  }
}
```

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_engaging_with_various_reactive_techniques_interacting_with_holographic_code_snippets_observables_and_Subjects_Angular_galaxy_backdrop_with_a_Star_Wars_atmosphere_Developer_hero_navigating_through_an_Angula-8.png" data-background-opacity="0.5" -->

> "Become one with the reactive Force, young Padawan." 

---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_engaging_with_various_reactive_techniques_interacting_with_holographic_code_snippets_observables_and_Subjects_Angular_galaxy_backdrop_with_a_Star_Wars_atmosphere_Developer_hero_navigating_through_an_Angula-9.png" data-background-opacity="0.5" -->

# The Ordeal
Implementing RxAngular Features

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_engaging_with_various_reactive_techniques_interacting_with_holographic_code_snippets_observables_and_Subjects_Angular_galaxy_backdrop_with_a_Star_Wars_atmosphere_Developer_hero_navigating_through_an_Angula-9.png" data-background-opacity="0.5" -->

### What's in there?

`PushPipe`: drop-in replacement of `AsyncPipe` with sub-view level rendering.

```html
<div>{{ forceStatus$ | push }}</div>
```
---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_engaging_with_various_reactive_techniques_interacting_with_holographic_code_snippets_observables_and_Subjects_Angular_galaxy_backdrop_with_a_Star_Wars_atmosphere_Developer_hero_navigating_through_an_Angula-9.png" data-background-opacity="0.5" -->

### What's in there?

`*rxLet`: just define a variable without the *truthy* shenanigans you're used to

```html
<ng-container *rxLet="forceStatus$; let forceStatus">
  <div>{{ forceStatus }}</div>
</ng-container>
```

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_engaging_with_various_reactive_techniques_interacting_with_holographic_code_snippets_observables_and_Subjects_Angular_galaxy_backdrop_with_a_Star_Wars_atmosphere_Developer_hero_navigating_through_an_Angula-9.png" data-background-opacity="0.5" -->

### What's in there?

`*rxIf`: an if with powers

```html
<ng-container *rxIf="jedi$; let jedi; suspense: loading; error: error">
  {{ jedi.name }} has joined the battle!
</ng-container>

<ng-template #loading>Loading... summoning Jedi powers...</ng-template>
<ng-template #error>Error: disturbance in the force!</ng-template>
```

```ts
jedi$ = of({ name: 'Obi-Wan Codenobi' }).pipe(
  delay(1000),
  catchError(() => throwError('disturbance in the force!'))
);
```

---
<!-- .slide: data-auto-animate -->
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_engaging_with_various_reactive_techniques_interacting_with_holographic_code_snippets_observables_and_Subjects_Angular_galaxy_backdrop_with_a_Star_Wars_atmosphere_Developer_hero_navigating_through_an_Angula-9.png" data-background-opacity="0.5" -->

### What's in there?

`*rxFor`: iterate with better change detection

```html
<ul>
  <li *rxFor="let item of items$; trackBy: trackById">{{ item.name }}</li>
</ul>
```

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_looking_at_holographic_projection_of_wise_mentor_figure_video_message_discovering_reactive_programming_techniques_space_odyssey_with_Star_Wars_inspired_elements_dynamic_lighting_digital_concept_art_inspire-2.png" data-background-opacity="0.5" -->

> "Fear of reactive programming leads to anger; anger leads to hate; hate leads to suffering. Don't be afraid, embrace it!"

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_returning_to_their_home_planet_celebrating_the_victory_over_non-reactive_programming_Angular_galaxy_glowing_brightly_in_the_background_Star_Wars-inspired_fireworks_and_a_sense_of_accomplishment_discovering-6.png" data-background-opacity="0.5" -->

#### Act 3
# Return
Bringing It All Together

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_returning_to_their_home_planet_celebrating_the_victory_over_non-reactive_programming_Angular_galaxy_glowing_brightly_in_the_background_Star_Wars-inspired_fireworks_and_a_sense_of_accomplishment_discovering-6.png" data-background-opacity="0.5" -->

## The Reward
Refactoring a Component

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_returning_to_their_home_planet_celebrating_the_victory_over_non-reactive_programming_Angular_galaxy_glowing_brightly_in_the_background_Star_Wars-inspired_fireworks_and_a_sense_of_accomplishment_discovering-6.png" data-background-opacity="0.5" -->

The Imperative Way

```ts
@Component({
  selector: 'app-imperative',
  template: `
    <button (click)="handleButtonClick()">Fetch Data</button>
    <div *ngFor="let item of data">{{item.name}}</div>
  `,
})
export class ImperativeComponent {
  data: any[] = [];

  handleButtonClick() {
    fetch('https://api.example.com/data')
      .then((response) => response.json())
      .then((fetchedData) => {
        this.data = fetchedData.map((item) => ({
          ...item,
          name: item.name.toUpperCase(),
        }));
      });
  }
}
```

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_returning_to_their_home_planet_celebrating_the_victory_over_non-reactive_programming_Angular_galaxy_glowing_brightly_in_the_background_Star_Wars-inspired_fireworks_and_a_sense_of_accomplishment_discovering-6.png" data-background-opacity="0.5" -->

## Solution

The Reactive Way

```ts
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { map, startWith, switchMap, tap } from 'rxjs/operators';
import { Subject } from 'rxjs';

@Component({
  selector: 'app-reactive',
  template: `
    <button (click)="fetchData.next()">Fetch Data</button>
    <div *rxFor="let item of data$">{{item.name}}</div>
  `,
})
export class ReactiveComponent {
  fetchData = new Subject<void>();

  data$ = this.fetchData.pipe(
    startWith(null),
    switchMap(() => this.http.get<any[]>('https://api.example.com/data')),
    map((fetchedData) =>
      fetchedData.map((item) => ({ ...item, name: item.name.toUpperCase() }))
    ),
    tap((data) => console.log('Data fetched:', data))
  );

  constructor(private http: HttpClient) {}
}
```

---

\*rx directives handle subscription

Events as Subjects allow us to reuse it without refactoring

`switchMap` handles complicated stuff: cancelling requests

---
<!-- slide bg="https://www.wired.com/images_blogs/threatlevel/2012/12/death-star.jpg" data-background-opacity="0.5" -->

What your code looks like if you follow the imperial way

---

## Share data!
plz don't do the same api call 9000 times within 500ms

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { ReplaySubject } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private dataSubject = new ReplaySubject<any[]>(1);
  data$ = this.dataSubject.asObservable();

  constructor(private http: HttpClient) {}

  fetchData() {
    this.http
      .get<any[]>('https://api.example.com/data')
      .pipe(
        tap((data) => {
          this.dataSubject.next(data);
        })
      )
      .subscribe();
  }
}
```

---

```ts
import { Component } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-component-a',
  template: `
    <button (click)="dataService.fetchData()">Fetch Data</button>
    <ng-container *rxLet="dataService.data$; let data">
      <div *ngFor="let item of data">{{item.name}}</div>
    </ng-container>
  `,
})
export class ComponentA {
  constructor(public dataService: DataService) {}
}
```

---

wait did we just create state management

---
<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Developer_hero_facing_a_series_of_challenges_represented_by_React_and_Angular_planets_neon_holograms_illustrating_performance_comparisons_Star_Wars-inspired_space_environment_Developer_hero_navigating_through_an_Angular_-2.png" data-background-opacity="0.5" -->

## The Return: A New World with RxAngular

Enhanced performance

Better maintainability

Clear side effects

Improved code readability

Easier refactoring

notes:

1. Enhanced performance: RxAngular leverages the power of reactive programming and Angular's OnPush change detection strategy to optimize rendering performance. This results in faster, more efficient applications with improved user experience.
2. Better maintainability: By using RxAngular, developers can write more declarative and less error-prone code. This leads to improved code readability, easier debugging, and fewer bugs.
3. Clear side effects management: RxAngular encourages a more structured approach to handling side effects, making it simpler to understand and manage them.
4. Simplified state management: Reactive programming enables developers to manage state in a more streamlined and elegant manner, reducing the need for complex state management libraries or patterns.
5. Improved code reusability: By adhering to reactive programming principles and using RxAngular, developers can create more modular and reusable components, making it easier to share code across different parts of an application or even across multiple projects.
6. Easier refactoring: RxAngular's reactive approach leads to a more explicit and predictable data flow, making it simpler to refactor components and reducing the risk of introducing bugs during the refactoring process.

By adopting RxAngular, developers can transform their Angular projects into high-performance, maintainable applications that are easier to work with, understand, and debug. This shift not only benefits the developers themselves but also leads to better user experiences and overall application quality.

---

<!-- slide bg="https://nienormaal.s3.eu-central-1.wasabisys.com/public/everything-is-a-stream/sd-Young_developer_battling_code_bugs_galaxy-inspired_background_futuristic_sci-fi_setting_dramatic_lighting_Star_Wars-inspired_digital_painting_high_definition_Developer_hero_navigating_through_an_Angular_galaxy_discoverin-6.png" -->