<style>
        table.weaponTable {
            border: 1px solid black;
            font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
        }

        .weaponTable td, .weaponTable th {
            padding: 0.5em;
            margin: 0;
            white-space: nowrap;
            text-align: right;
        }

        .weaponTable th {
            background-color: rgb(225, 225, 230);
            font-weight: normal;
            vertical-align: bottom;
        }

        .weaponTable .name {
            width: 14em;
        }

        .weaponTable .number {
            width: 1.5em;
            text-align: right;
        }

        .weaponTable .notes {
            width: 8em;
            text-align: left;
        }

    </style>

Hand weapons in suits do a variable amount of damage based on the users' Body. They also have a variety of other statistics:

### Hands

The number of hands required. Most weapons can be used one handed it requires another round of windup, and its Min Body increases by 50%(min 1). You cannot use a weapon one handed if your Body is below its minimum Body. 

### Ready

The number of rounds after you use the weapon that you have to spend preparing the weapon for use. An unready weapon can still be used to parry. 

### Min Strength

The minimum required Body to use the weapon. You can use a weapon that is too heavy for you but you must spend one round winding it up each time you use it. 

### Reach

The meters between you and your target at which you can strike your target. At reach 0 you must be adjacent to your target to strike them; a weapon of reach 1 can strike a target one meter away. Reach also adds to your defense, at a cost.

#### Optional: Reach Defense Bonus

The difference between your reach and your targets' reach adds to your defense; however once you are hit, your defense goes down by the same amount (as they are "inside your reach") until either you hit your opponent or you successfully parry one of their attacks - and you must retreat one meter that round.  

### Notes

#### Polearm

Polearms cannot attack unless the target is at least 2 meters away; however they can be used as a staff when target is within reach. 

#### Windup

Weapon takes one round of preparation to use at full strength before it attacks. You can attack without a windup, but you will do half damage (round down). 

#### Barbed

Weapon takes a round after a successful attack to extract from target. 

<div ng-controller="WeaponCtrl" class="ng-scope">
    <h1>SUITS Weapons</h1>

    <!-- ngRepeat: file in files --><div ng-repeat="file in files" class="ng-scope">
        <h2 class="ng-binding">Sheet 1-Hand Weapons - Swords.csv</h2>
        <table class="weaponTable" cellpadding="0" border="0" cellspacing="0">
            <thead>

            <tr>
                <th rowspan="2" class="name">Name</th>
                <th rowspan="2">Penetration</th>
                <th rowspan="2">Min<br>Body</th>
                <th colspan="10">Damage, based on Body</th>
                <th rowspan="2">Hands</th>
                <th rowspan="2">Reach</th>
                <th class="notes" rowspan="2">notes</th>
            </tr>
            <tr>
                <th class="number">1</th>
                <th class="number">2</th>
                <th class="number">3</th>
                <th class="number">4</th>
                <th class="number">5</th>
                <th class="number">6</th>
                <th class="number">7</th>
                <th class="number">8</th>
                <th class="number">9</th>
                <th class="number">10</th>

            </tr>
            </thead>

            <tbody>
            <!-- ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Machete</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Shortsword</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">+1 defense</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Broadsword</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">3</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">+1 defense</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Claymore</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">4</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">11</td>
                <td class="number ng-binding">13</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">+1 defense</td>
            </tr><!-- end ngRepeat: weapon in file.weapons -->
            </tbody>
        </table>
    </div><!-- end ngRepeat: file in files --><div ng-repeat="file in files" class="ng-scope">
        <h2 class="ng-binding">Sheet 1-Hand Weapons - small.csv</h2>
        <table class="weaponTable" cellpadding="0" border="0" cellspacing="0">
            <thead>

            <tr>
                <th rowspan="2" class="name">Name</th>
                <th rowspan="2">Penetration</th>
                <th rowspan="2">Min<br>Body</th>
                <th colspan="10">Damage, based on Body</th>
                <th rowspan="2">Hands</th>
                <th rowspan="2">Reach</th>
                <th class="notes" rowspan="2">notes</th>
            </tr>
            <tr>
                <th class="number">1</th>
                <th class="number">2</th>
                <th class="number">3</th>
                <th class="number">4</th>
                <th class="number">5</th>
                <th class="number">6</th>
                <th class="number">7</th>
                <th class="number">8</th>
                <th class="number">9</th>
                <th class="number">10</th>

            </tr>
            </thead>

            <tbody>
            <!-- ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Hand to Hand</td>
                <td class="ng-binding">-1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Knife</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons -->
            </tbody>
        </table>
    </div><!-- end ngRepeat: file in files --><div ng-repeat="file in files" class="ng-scope">
        <h2 class="ng-binding">Sheet 1-Hand Weapons - pole.csv</h2>
        <table class="weaponTable" cellpadding="0" border="0" cellspacing="0">
            <thead>

            <tr>
                <th rowspan="2" class="name">Name</th>
                <th rowspan="2">Penetration</th>
                <th rowspan="2">Min<br>Body</th>
                <th colspan="10">Damage, based on Body</th>
                <th rowspan="2">Hands</th>
                <th rowspan="2">Reach</th>
                <th class="notes" rowspan="2">notes</th>
            </tr>
            <tr>
                <th class="number">1</th>
                <th class="number">2</th>
                <th class="number">3</th>
                <th class="number">4</th>
                <th class="number">5</th>
                <th class="number">6</th>
                <th class="number">7</th>
                <th class="number">8</th>
                <th class="number">9</th>
                <th class="number">10</th>

            </tr>
            </thead>

            <tbody>
            <!-- ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Staff</td>
                <td class="ng-binding">-1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Spear</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">polearm</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Trident</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">barbed<br> polearm</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Pike</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">3</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">polearm</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Poleaxe</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">4</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">12</td>
                <td class="number ng-binding">13</td>
                <td class="number ng-binding">14</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">windup<br> polearm</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Halberd</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">4</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">12</td>
                <td class="number ng-binding">13</td>
                <td class="number ng-binding">15</td>
                <td class="number ng-binding">16</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">3</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">windup<br> polearm<br> can be used as spear</td>
            </tr><!-- end ngRepeat: weapon in file.weapons -->
            </tbody>
        </table>
    </div><!-- end ngRepeat: file in files --><div ng-repeat="file in files" class="ng-scope">
        <h2 class="ng-binding">Sheet 1-Hand Weapons - Axes and Clubs.csv</h2>
        <table class="weaponTable" cellpadding="0" border="0" cellspacing="0">
            <thead>

            <tr>
                <th rowspan="2" class="name">Name</th>
                <th rowspan="2">Penetration</th>
                <th rowspan="2">Min<br>Body</th>
                <th colspan="10">Damage, based on Body</th>
                <th rowspan="2">Hands</th>
                <th rowspan="2">Reach</th>
                <th class="notes" rowspan="2">notes</th>
            </tr>
            <tr>
                <th class="number">1</th>
                <th class="number">2</th>
                <th class="number">3</th>
                <th class="number">4</th>
                <th class="number">5</th>
                <th class="number">6</th>
                <th class="number">7</th>
                <th class="number">8</th>
                <th class="number">9</th>
                <th class="number">10</th>

            </tr>
            </thead>

            <tbody>
            <!-- ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Club</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Hammer</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Pick</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">barbed</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Mace</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">11</td>
                <td class="number ng-binding">12</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">AP1</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Warhammer</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">4</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">11</td>
                <td class="number ng-binding">12</td>
                <td class="number ng-binding">13</td>
                <td class="number ng-binding">15</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">AP1</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Flail</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">12</td>
                <td class="number ng-binding">13</td>
                <td class="number ng-binding">15</td>
                <td class="number ng-binding">16</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">windup<br> barbed</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Hatchet</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Axe</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">3</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">11</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Battleaxe</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">4</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">11</td>
                <td class="number ng-binding">12</td>
                <td class="number ng-binding">14</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons -->
            </tbody>
        </table>
    </div><!-- end ngRepeat: file in files --><div ng-repeat="file in files" class="ng-scope">
        <h2 class="ng-binding">Sheet 1-Hand Weapons - Ancient - small.csv</h2>
        <table class="weaponTable" cellpadding="0" border="0" cellspacing="0">
            <thead>

            <tr>
                <th rowspan="2" class="name">Name</th>
                <th rowspan="2">Penetration</th>
                <th rowspan="2">Min<br>Body</th>
                <th colspan="10">Damage, based on Body</th>
                <th rowspan="2">Hands</th>
                <th rowspan="2">Reach</th>
                <th class="notes" rowspan="2">notes</th>
            </tr>
            <tr>
                <th class="number">1</th>
                <th class="number">2</th>
                <th class="number">3</th>
                <th class="number">4</th>
                <th class="number">5</th>
                <th class="number">6</th>
                <th class="number">7</th>
                <th class="number">8</th>
                <th class="number">9</th>
                <th class="number">10</th>

            </tr>
            </thead>

            <tbody>
            <!-- ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Hand to Hand</td>
                <td class="ng-binding">-1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Wood knife</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Obsidian Knife</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">6</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons -->
            </tbody>
        </table>
    </div><!-- end ngRepeat: file in files --><div ng-repeat="file in files" class="ng-scope">
        <h2 class="ng-binding">Sheet 1-Hand Weapons - Ancient - pole.csv</h2>
        <table class="weaponTable" cellpadding="0" border="0" cellspacing="0">
            <thead>

            <tr>
                <th rowspan="2" class="name">Name</th>
                <th rowspan="2">Penetration</th>
                <th rowspan="2">Min<br>Body</th>
                <th colspan="10">Damage, based on Body</th>
                <th rowspan="2">Hands</th>
                <th rowspan="2">Reach</th>
                <th class="notes" rowspan="2">notes</th>
            </tr>
            <tr>
                <th class="number">1</th>
                <th class="number">2</th>
                <th class="number">3</th>
                <th class="number">4</th>
                <th class="number">5</th>
                <th class="number">6</th>
                <th class="number">7</th>
                <th class="number">8</th>
                <th class="number">9</th>
                <th class="number">10</th>

            </tr>
            </thead>

            <tbody>
            <!-- ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Staff</td>
                <td class="ng-binding">-1</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Wood Spear</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">polearm</td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Obsidian Spear</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes">polearm</td>
            </tr><!-- end ngRepeat: weapon in file.weapons -->
            </tbody>
        </table>
    </div><!-- end ngRepeat: file in files --><div ng-repeat="file in files" class="ng-scope">
        <h2 class="ng-binding">Sheet 1-Hand Weapons - Ancient - Axes and Clubs.csv</h2>
        <table class="weaponTable" cellpadding="0" border="0" cellspacing="0">
            <thead>

            <tr>
                <th rowspan="2" class="name">Name</th>
                <th rowspan="2">Penetration</th>
                <th rowspan="2">Min<br>Body</th>
                <th colspan="10">Damage, based on Body</th>
                <th rowspan="2">Hands</th>
                <th rowspan="2">Reach</th>
                <th class="notes" rowspan="2">notes</th>
            </tr>
            <tr>
                <th class="number">1</th>
                <th class="number">2</th>
                <th class="number">3</th>
                <th class="number">4</th>
                <th class="number">5</th>
                <th class="number">6</th>
                <th class="number">7</th>
                <th class="number">8</th>
                <th class="number">9</th>
                <th class="number">10</th>

            </tr>
            </thead>

            <tbody>
            <!-- ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Club</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">5</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">0</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Stone Club</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Stone War Club</td>
                <td class="ng-binding">0</td>
                <td class="ng-binding">3</td>
                <td class="number ng-binding">1</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">11</td>
                <td class="number ng-binding">13</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Obsidian Hatchet</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">2</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Obsidian Axe</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">3</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">3</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">7</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">11</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">1</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons --><tr ng-repeat="weapon in file.weapons" class="ng-scope">
                <td class="name ng-binding">Obsidian War Axe</td>
                <td class="ng-binding">1</td>
                <td class="ng-binding">4</td>
                <td class="number ng-binding">2</td>
                <td class="number ng-binding">4</td>
                <td class="number ng-binding">5</td>
                <td class="number ng-binding">6</td>
                <td class="number ng-binding">8</td>
                <td class="number ng-binding">9</td>
                <td class="number ng-binding">10</td>
                <td class="number ng-binding">12</td>
                <td class="number ng-binding">13</td>
                <td class="number ng-binding">14</td>
                <td class="ng-binding">2</td>
                <td class="ng-binding">2</td>
                <td class="notes ng-binding" ng-bind-html="weapon.notes"></td>
            </tr><!-- end ngRepeat: weapon in file.weapons -->
            </tbody>
        </table>
    </div><!-- end ngRepeat: file in files -->
</div>