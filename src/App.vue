<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-9">
                <img src="img/sword.png" alt="Суммарный урон всех Богов" title="Суммарный урон всех Богов"><p title="Суммарный урон всех Богов" class="gods-damage">{{ godsDamage }}</p>
                <span v-for="monster of currentMonster" :key="monster.name">
                    <div class="background">
                        <div class="row">
                            <div class="col-lg-4 offset-lg-4">
                                <h1 class="text-center monster-name">{{ monster.name }}</h1>
                                <img :src="monster.img" width="500px" alt="cerberus" class="monster" @click="clickOnMonster"/>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-lg-4 offset-lg-4">
                                <div class="progress" style="width: 100%; margin-top: 100px">
                                    <div class="progress-bar bg-danger" :style="'width: '+monster.currentHpPercent+'%'"
                                         role="progressbar"  aria-valuenow="25" aria-valuemin="0" aria-valuemax="100">
                                        {{ monster.currentHp }}/{{ monster.hp }}
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </span>
            </div>
            <div class="col-lg-3">
                <div class="row">
                    <img width="70px" alt="faith" src="img/faith_w.png"><p class="faith">{{ faith }}</p>
                    <img width="70px" alt="glory" src="img/glory_w.png" style="margin-left: 4vh"><p class="glory">{{ glory }}</p>
                </div>
                <div class="row">
                    <button class="god" v-for="(god, name) of gods" :key="god.damage" type="button" :style="'background: url(img/gods/'+name+'.jpg)'" @click="clickOnGod(god)"></button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import _ from 'lodash';
    export default {
        name: 'app',
        data() {
            return {
                faith: 0,
                glory: 0,
                gods: {
                    aid: {
                        damage: 1,
                        cost: 1,
                        count: 0,
                    },
                    artemida: {
                        damage: 2,
                        cost: 2,
                        count: 0,
                    },
                    brahma: {
                        damage: 3,
                        cost: 3,
                        count: 0,
                    },
                    kali: {
                        damage: 4,
                        cost: 4,
                        count: 0
                    },
                    odin: {
                        damage: 5,
                        cost: 5,
                        count: 0
                    }
                },
                monsters: [{
                    name: 'Цербер',
                    hp: 100,
                    currentHp: 100,
                    currentHpPercent: 100,
                    reward: 10,
                    img: 'img/monsters/cerberus.png'
                },{
                    name: 'Гигант',
                    hp: 500,
                    currentHp: 500,
                    currentHpPercent: 100,
                    reward: 50,
                    img: 'img/monsters/giant.png'
                },{
                    name: 'Крампус',
                    hp: 700,
                    currentHp: 700,
                    currentHpPercent: 100,
                    reward: 70,
                    img: 'img/monsters/krampus.png'
                }],
                currentMonster: {
                    monster: {
                        name: 'Цербер',
                        hp: 100,
                        currentHp: 100,
                        currentHpPercent: 100,
                        reward: 10,
                        img: 'img/cerberus.png'
                    }
                },
                clickDamage: 100,
                godsDamage: 0
            }
        },
        watch: {
            'currentMonster.monster.currentHp': function (value) {
                this.currentMonster.monster.currentHpPercent = value / this.currentMonster.monster.hp * 100;
                if (value <= 0) {
                    this.faith += this.currentMonster.monster.reward;
                    this.changeMonster();
                }
            }
        },
        created: function () {
            let self = this;
            setInterval(function () {
                self.currentMonster.monster.currentHp -= self.godsDamage;
            }, 1000)
        },
        methods: {
            clickOnMonster() {
                if (this.currentMonster.monster.currentHp >= 0) {
                    this.currentMonster.monster.currentHp -= this.clickDamage;
                }
            },
            clickOnGod(god) {
                if (this.faith >= god.cost) {
                    this.faith -= god.cost;
                    this.godsDamage += god.damage;
                    god.count += 1;
                }
            },
            changeMonster() {
                let randomMonsterIndex = _.random(0, this.monsters.length-1);
                Object.assign(this.currentMonster.monster, this.monsters[randomMonsterIndex]);
            }
        }
    }
</script>

<style>

</style>
