<template>
    <div class="container">
        <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">基礎戰力計算</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <p>目前基礎戰力為 <span class="badge bg-dark">{{ base }}</span></p>
                        <form>
                            <div class="mb-3">
                                <label for="customRange3" class="form-label">競技場階級 : {{ level }}</label>
                                <input type="range" class="form-range" min="1" max="10" step="1" v-model="level" @change="base_cal"/>
                            </div>
                            <div class="mb-3">
                                <label for="customRange3" class="form-label">加成角色數量 : {{ special }}</label>
                                <input type="range" class="form-range" min="0" max="6" step="1" v-model="special" @change="base_cal"/>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
        <form>
            <div class="row">
                <div class="col">
                    <div class="mb-3">
                        <label class="form-label">
                            基礎戰力
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-question-circle" viewBox="0 0 16 16" data-bs-toggle="modal" data-bs-target="#staticBackdrop" href="">
                                <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                                <path d="M5.255 5.786a.237.237 0 0 0 .241.247h.825c.138 0 .248-.113.266-.25.09-.656.54-1.134 1.342-1.134.686 0 1.314.343 1.314 1.168 0 .635-.374.927-.965 1.371-.673.489-1.206 1.06-1.168 1.987l.003.217a.25.25 0 0 0 .25.246h.811a.25.25 0 0 0 .25-.25v-.105c0-.718.273-.927 1.01-1.486.609-.463 1.244-.977 1.244-2.056 0-1.511-1.276-2.241-2.673-2.241-1.267 0-2.655.59-2.75 2.286zm1.557 5.763c0 .533.425.927 1.01.927.609 0 1.028-.394 1.028-.927 0-.552-.42-.94-1.029-.94-.584 0-1.009.388-1.009.94z"/>
                            </svg>
                        </label>
                        <input type="number" class="form-control" placeholder="基礎戰力" v-model="base" />
                    </div>
                </div>
                <div class="col">
                    <div class="mb-3">
                        <label class="form-label">技能合計cd數</label>
                        <input type="number" class="form-control" placeholder="合計cd數" v-model="cd" />
                    </div>
                </div>
            </div>
            <div class="mb-3">
                <label for="customRange3" class="form-label">回合 : {{ round }}</label>
                <input type="range" class="form-range" min="5" max="15" step="1" v-model="round" />
            </div>
            <div class="mb-3">
                <label for="customRange3" class="form-label" >傷害 : {{ get_damage }}</label>
                <input type="range" class="form-range" min="100" max="10000" step="50" v-model="damage" />
            </div>
            <div class="mb-3">
                <label for="customRange3" class="form-label">時間 : {{ get_time }}</label>
                <input type="range" class="form-range" min="40" max="360" step="1" v-model="time"/>
            </div>
            <div class="mb-3">
                <label for="customRange3" class="form-label">combo : {{ combo }}C</label>
                <input type="range" class="form-range" min="1" max="25" step="1" v-model="combo"/>
            </div>
        </form>
        <div class="alert alert-primary" role="alert">分數：{{ get_score }}</div>
    </div>
</template>

<script>
export default {
    name:'Page',
    data(){
        return{
            base:2320,
            cd:0,
            round:5,
            damage:10000,
            time:132,
            combo:25,
            level:10,
            special:6
        }
    },
    methods:{
        pad(width, string, padding) { 
            return (width <= string.length) ? string : this.pad(width, padding + string, padding)
        },
        base_cal: function(){
            this.base = Math.round((950 + (this.level*50))*(1 + (0.1*this.special)))
        }
    },
    computed:{
        get_damage: function(){
            if(this.damage == 10000)
                return "1億";
            else
                return this.damage+"萬";
        },
        get_time: function(){
            return this.pad(2,Math.floor(this.time/60).toString(), '0') + ":" + this.pad(2,(this.time%60).toString(), '0')
        },
        get_score: function(){
            var score = 100;
            if(this.time > 120){
                var punish = Math.ceil((this.time-120)/12)*25
                score += 500-punish;
            }else{
                score += (500 + (120-this.time));
            }

            if(this.damage <= 2150){
                if(Math.ceil((2150 - this.damage)/100)*25 <= 500)
                    score += 500 - Math.ceil((2150 - this.damage)/100)*25;
            }else{
                score += 500 + Math.floor((this.damage - 2000)/100);
            }
            
            if(1000-(this.round-5)*100 >= 0){
                score += 1000-(this.round-5)*100;
            }
            
            if(200 - (25-this.combo)*10 >= 0){
                score += 200 - (25-this.combo)*10;
            }

            return score + parseInt(this.base) - (this.cd*10);
        }
    }
}
</script>