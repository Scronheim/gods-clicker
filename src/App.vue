<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-9">
                <img src="img/sword.png" title="Суммарный урон всех Богов">
                <p title="Суммарный урон всех Богов" class="gods-damage">{{ convertGodsDamage }}</p><p class="current-zone">Уровень: {{ currentZone }}</p> <button class="btn btn-danger" @click="stopTimer()">Stop</button>
                <span v-for="monster of currentMonster" :key="monster.name">
                    <div class="background">
                        <div class="row">
                            <div class="col-lg-4 offset-lg-4">
                                <h1 class="text-center monster-name">{{ monster.title }}</h1>
                                <img v-cloak :src="monster.img" width="500px" height="600px" alt="cerberus" class="monster" @click="clickOnMonster"/>
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
                    <img width="70px" alt="faith" src="img/faith_w.png"><p class="faith">{{ convertFaith }}</p>
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
    import numeral from 'numeral';
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
                    },
                    shiva: {
                        damage: 6,
                        cost: 6,
                        count: 0
                    },
                    zeus: {
                        damage: 7,
                        cost: 7,
                        count: 0
                    },
                    veles: {
                        damage: 8,
                        cost: 8,
                        count: 0
                    },
                    skadi: {
                        damage: 9,
                        cost: 9,
                        count: 0
                    },
                    ra: {
                        damage: 10,
                        cost: 10,
                        count: 0
                    },
                    osiris: {
                        damage: 11,
                        cost: 11,
                        count: 0
                    }
                },
                monsters: [{
                    title: 'Цербер',
                    hp: 100,
                    currentHp: 100,
                    currentHpPercent: 100,
                    reward: 10,
                    img: 'img/monsters/cerberus.png'
                },{
                    title: 'Гигант',
                    hp: 500,
                    currentHp: 500,
                    currentHpPercent: 100,
                    reward: 50,
                    img: 'img/monsters/giant.png'
                },{
                    title: 'Крампус',
                    hp: 700,
                    currentHp: 700,
                    currentHpPercent: 100,
                    reward: 70,
                    img: 'img/monsters/krampus.png'
                },{
                    title: 'Призрак',
                    hp: 1000,
                    currentHp: 1000,
                    currentHpPercent: 100,
                    reward: 150,
                    img: 'img/monsters/ghost.png'
                },{
                    title: 'Ракшас',
                    hp: 1300,
                    currentHp: 1300,
                    currentHpPercent: 100,
                    reward: 190,
                    img: 'img/monsters/rakshas.png'
                },{
                    title: 'Медуза',
                    hp: 1500,
                    currentHp: 1500,
                    currentHpPercent: 100,
                    reward: 220,
                    img: 'img/monsters/medusa.png'
                },{
                    title: 'Повелитель душ',
                    hp: 1700,
                    currentHp: 1700,
                    currentHpPercent: 100,
                    reward: 270,
                    img: 'img/monsters/lord-of-souls.png'
                },{
                    title: 'Джин',
                    hp: 2000,
                    currentHp: 2000,
                    currentHpPercent: 100,
                    reward: 350,
                    img: 'img/monsters/gin.png'
                }
                ],
                currentMonster: {
                    monster: {
                        title: 'Цербер',
                        hp: 100,
                        currentHp: 100,
                        currentHpPercent: 100,
                        reward: 10,
                        img: 'img/cerberus.png'
                    }
                },
                currentZone: 8,
                clickDamage: 100,
                godsDamage: 0,
                damageTimer: '',
                bossHpMultiplier: 10
            }
        },
        computed: {
            convertGodsDamage() {
                return numeral(this.godsDamage).format('0.00a');
            },
            convertFaith() {
                return numeral(this.faith).format('0.00a');
            }
        },
        watch: {
            'currentMonster.monster.currentHp': function (value) {
                this.currentMonster.monster.currentHpPercent = value / this.currentMonster.monster.hp * 100;
                if (value <= 0) {
                    this.faith += this.currentMonster.monster.reward;
                    this.changeMonster();
                }
            },
            'currentZone' : function (value) {
                if ((value) % 10 === 0) {
                    this.currentMonster.monster.hp *= this.bossHpMultiplier;
                    this.currentMonster.monster.currentHp *= this.bossHpMultiplier;
                    this.currentMonster.monster.title = 'Босс - ' + this.currentMonster.monster.title;
                }
            }
        },
        updated() {
            localStorage.setItem('faith', this.faith);
            localStorage.setItem('glory', this.glory);
            localStorage.setItem('godsDamage', this.godsDamage);
            localStorage.setItem('clickDamage', this.clickDamage);
            localStorage.setItem('currentZone', this.currentZone);
        },
        created() {
            let self = this;
            if (localStorage.getItem('faith')) {
                this.faith = parseInt(localStorage.getItem('faith'));
                this.glory = parseInt(localStorage.getItem('glory'));
                this.godsDamage = parseInt(localStorage.getItem('godsDamage'));
                this.clickDamage = parseInt(localStorage.getItem('clickDamage'));
                this.currentZone = parseInt(localStorage.getItem('currentZone'));
            } else {
                localStorage.setItem('faith', 0);
                localStorage.setItem('glory', 0);
                localStorage.setItem('godsDamage', 0);
                localStorage.setItem('clickDamage', this.clickDamage);
                localStorage.setItem('currentZone', this.currentZone);
            }
            let randomMonsterIndex = _.random(0, this.monsters.length-1);
            Object.assign(this.currentMonster.monster, this.monsters[randomMonsterIndex]);
            // this.changeMonster();
            this.damageTimer = setInterval(function () {
                self.currentMonster.monster.currentHp -= self.godsDamage;
            }, 1000);
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
                this.changeZone();
            },
            stopTimer() {
                clearInterval(this.damageTimer);
            },
            changeZone() {
                this.currentZone += 1;
            }
        }
    }
</script>

<style>

</style>
