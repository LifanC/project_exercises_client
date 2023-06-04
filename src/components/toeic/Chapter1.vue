<script setup>

</script>

<template>
  <!--  1-->
  <el-divider/>
  <el-text>Chapter1</el-text>
  <el-descriptions
      title="Point(1) S + Vi"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞 + 不及物動詞" :span="2">S + Vi +</el-descriptions-item>
    <el-descriptions-item label="(形容詞)(介系詞 + 名詞)">(adj.)<br>(prep. + N)</el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.The cildren laughed.(孩子們笑了。)
      <br>
      2.The cildren laughed loudly.(孩子們大聲地笑著。)
      <br>
      3.The cildren laughed loudly in the backyard.(孩子們在後院裡大聲地笑著。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  2-->
  <el-divider/>
  <el-descriptions
      title="Point(2) S + Vi + SC"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞 + 不及物動詞" :span="2">S + Vi +</el-descriptions-item>
    <el-descriptions-item label="主詞補語">SC</el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.My father is an architect.(我的父親是位建築師。)
      <br>
      2.The customers seem satisfied.(顧客們似乎感到滿意。)
      <br>
      3.The spaghetti tastes delicious.(這道義大利麵嚐起來很美味。)
      <br>
      4.He remained calm in the face of danger.(面臨危險時他乃保持冷靜。)
      <br>
      5.The baby fell asleep in her father's arms.(寶寶在他父親的臂彎裡睡著了。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  延伸教學:連綴動詞用法-->
  <el-divider/>
  <el-descriptions
      title="延伸教學:連綴動詞用法"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞" :row="2">S +</el-descriptions-item>
    <el-descriptions-item label="不及物動詞(Vi)">
      <br>
      似乎: seem , appear
      <br>
      感官: feel , look , smell , taste , sound
      <el-divider/>
      保持: keep , stay , remain
      <br>
      變成: become , get , go , grow , turn , fall
    </el-descriptions-item>
    <el-descriptions-item>
      + adj.
      <br>
      + like N(像......)
      <el-divider/>
      + adj.
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.The boy seems happy.(這個男孩似乎很快樂。)
      <br>
      2.The cake tastes sweet.(這個蛋糕嚐起來很甜。)
      <br>
      3.Iris looks like her mother.(Iris看起來像她的母親。)
      <br>
      4.We should stay calm.(我們應該保持冷靜。)
      <br>
      5.It's getting cold.(天氣逐漸變冷。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  統整比較-->
  <el-divider/>
  <el-descriptions
      title="統整比較"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞 + 不及物動詞" :span="2">S + Vi +</el-descriptions-item>
    <el-descriptions-item label="(副詞)(形容詞)">
      (adv.)
      <br>
      (adj.)
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.S + Vi即可淒楚表達語意時，Vi後可加adv.。
      <br>
      2.S + Vi無法清楚表達語意時，Vi後須加adj.。
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.They are dancing.(他們正在跳舞。)&emsp;<b style="color: blue">S+Vi語意清楚</b>
      <br>
      2.They are dancing happily.(他們正在快樂地跳舞。)&emsp;<b style="color: blue">S+Vi語意清楚，加上adv.用來描述Vi的狀態</b>
      <br>
      3.<b style="color: blue">(錯誤)</b>The kids looked.(孩子們看起來。)&emsp;<b style="color: blue">S+Vi語意不清楚，且不符合文法</b>
      <br>
      4.<b style="color: blue">(正確)</b>The kids looked happy.(孩子們看起來很快樂。)&emsp;<b style="color: blue">S+Vi語意不清楚，必須加上adj.當作SC，補充說明S的狀態</b>
    </el-descriptions-item>
  </el-descriptions>
  <!--  考試-->
  <el-divider/>
  <el-descriptions
      title="Quiz Time"
      direction="vertical"
      border
  >
    <el-descriptions-item>
      <el-tag size="small">Quiz Time</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="考試題目">
      1.老太太悲傷地哭泣著。
      <br><b style="color: blue">The old lady <u>________</u> <u>________</u>.</b>
      <br>
      2.我女朋友似乎在生氣。
      <br><b style="color: blue">My girlfriend a <u>________</u> <u>________</u>.</b>
      <br>
      3.那杯飲料嚐起來像自來水。
      <br><b style="color: blue">The drink <u>________</u> <u>________</u> tap water.</b>
      <br>
      4.我們在圖書館裡應該保持安靜。
      <br><b style="color: blue">We should remain <u>________</u> in the library.</b>
      <br>
      <el-tag size="small">答</el-tag>
      1.cried/cries; sadly&emsp;2.appeared/appears; angry&emsp;3.tasted/tastes; like&emsp;4.quiet
    </el-descriptions-item>
  </el-descriptions>
  <!--  3-->
  <el-divider/>
  <el-descriptions
      title="Point(3) S + Vt + O"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞 + 及物動詞" :span="2">S + Vt +</el-descriptions-item>
    <el-descriptions-item label="受詞">O</el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.Vt需接O，全句語意才完整
      <br>
      2.O可略分為:<br>(1)N、代名詞<br>(2)Ving<br>(3)to VR(動詞原形)
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.The workers moved the furniture.(工人們搬動了家具。)
      <br>
      2.You should avoid making the same mistake.(你應該避免犯同樣的錯誤。)
      <br>
      3.Justin desires to study abroad.(Justin渴望出國讀書。)
      <br>
    </el-descriptions-item>
  </el-descriptions>
  <!--  4-->
  <el-divider/>
  <el-descriptions
      title="Point(4) S + Vt + O + OC"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞 + 及物動詞" :span="2">S + Vt +</el-descriptions-item>
    <el-descriptions-item label="受詞 + 受詞補語">O + OC</el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.OC可為 : N、to VR、adj、現代分詞(V-ing)、過去分詞(p.p)。
      <br>
      2.句型之動詞:<br>(1)含[指名]意味的動詞。<br>(2)感官動詞。<br>(3)使役動詞。<br>(4)find(發覺)、leave(使保持)、keep(使保持)
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.Mr.Smith has named his newborn baby Dan.(Smith先生為她的新生兒命名為Dan。)
      <br><b style="color: blue">名詞Dan補充修飾受詞his newborn baby。name含[指名]意味之動詞，用N作OC</b>
      <br>
      2.My father wants me to follow in his footsteps.(我的父親要我繼承她的事業。)
      <br><b style="color: blue">不定詞to follow in his footsteps補充修飾受詞me。</b>
      <br>
      3.His sudden death makes me sad.(她突如其來的死亡讓我悲傷。)
      <br><b style="color: blue">形容詞sad補充修飾受詞me。</b>
      <br>
      4.Brad heard someone knocking on he door.(Brad聽到有人在敲門。)
      <br><b style="color: blue">現代分詞knocking補充修飾受詞someone。</b>
      <br>
      5.Betty found her name-brand bag stolen.(Betty發現她的名牌包被偷了。)
      <br><b style="color: blue">過去分詞stolen補充修飾受詞her name-brand bag。</b>
    </el-descriptions-item>
  </el-descriptions>
  <!--  [指名]動詞用法-->
  <el-divider/>
  <el-descriptions
      title="[指名]動詞用法"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞" :row="2">S +</el-descriptions-item>
    <el-descriptions-item>
      [指派]: appoint(指派)、elect(推選)、vote(推選)
      <el-divider/>
      [命名]: name(命名)、nickname(取綽號)、call(稱呼)、label(標示為;稱作)
    </el-descriptions-item>
    <el-descriptions-item>
      + O + N
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.部分含有[指派]及[命名]意味之動詞，常用N當OC。
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.We elected John our leader.(我們推選John做我們隊長。)
      <br>
      2.Jennifer call her pet dog Spotty.(Jennifer叫她的小狗Spotty。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  感官動詞用法-->
  <el-divider/>
  <el-descriptions
      title="感官動詞用法"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞" :row="2">S +</el-descriptions-item>
    <el-descriptions-item>
      感官動詞 + O +
    </el-descriptions-item>
    <el-descriptions-item>
      VR(主動)
      <br>
      V-ing(主動且正在進行)
      <br>
      p.p(被動)
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.常見的感官動詞:<br>(1)see看到<br>(2)watch看到<br>(3)notice注意到<br>(4)observe觀察到<br>(5)hear聽到<br>(6)feel感覺到<br>等。
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.The witness saw an armed robber enter the bank.(目擊者看見一個武裝搶匪進入銀行。)
      <br><b style="color: blue">用原形動詞enter，表達主詞the witness目睹了整個搶匪進入銀行的過程。</b>
      <br>
      2.The witness saw an armed robber entering the bank.(目擊者看見一個武裝搶匪進入銀行。)
      <br><b style="color: blue">用現代分詞entering，表達主詞the witness看到搶匪時。該搶匪正進入銀行，但並未目睹全程。</b>
      <br>
      3.I noticed a few teardrops rolling down her cheeks.(我注意到有幾滴淚珠滾落她臉頰。)
      <br>
      4.I saw Kevin scolded by the teacher.(我看見Kevin被老師罵。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  考試-->
  <el-divider/>
  <el-descriptions
      title="Quiz Time"
      direction="vertical"
      border
  >
    <el-descriptions-item>
      <el-tag size="small">Quiz Time</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="考試題目">
      1.他們為Tom取綽號為小胖(Fatty)。
      <br><b style="color: blue">They <u>________</u>.</b>
      <br>
      2.我注意到老師正在盯著(stare at)我。
      <br><b style="color: blue">I <u>________</u>.</b>
      <br>
      3.學生們看到一個老人被一輛公車撞上。
      <br><b style="color: blue">The students <u>________</u>.</b>
      <br>
      <el-tag size="small">答</el-tag>
      1.nickname(d) Tom Fatty&emsp;2.notice(d) the teacher staring at me&emsp;3.saw the old man hit by a bus
    </el-descriptions-item>
  </el-descriptions>
  <!--  使役動詞用法一 : 以不同動詞形式作OC-->
  <el-divider/>
  <el-descriptions
      title="使役動詞用法一 : 以不同動詞形式作OC"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞" :span="2">S +</el-descriptions-item>
    <el-descriptions-item label="動詞 + 受詞">
      make + O + VR(主動) 或 p.p.(被動)
      <br>
      have + O + VR(主動) 或 p.p.(被動)
      <br>
      get + O + to V(主動) 或 p.p.(被動)
      <br>
      let + O + VR(主動) 或 be p.p.(被動)
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.以上使役動詞一般譯為[使]、[讓]、或[叫]，但:
      <br>
      (1)[迫使、要求]:較常用make,have。
      <br>
      (2)[說服]:較常用get。
      <br>
      (3)[容許]:較常用let。
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.You can never make me go against my will.(你絕對不可能逼我違背我自己的意願。)
      <br>
      2.I couldn't make myself heard. It was too noisy.(我無法讓人聽清楚我的聲音。實在太吵了。)
      <br>
      3.The teacher had the kids play an interesting game.(老師讓小朋友們玩一個有趣的遊戲。)
      <br>
      4.Mr.Cruise had the fence in his backyard painted.(Cruise先生叫人為後院的圍籬上漆。)
      <br>
      5.We need to get the work done by tomorrow.(明天前我們必須將工作完成。)
      <br>
      6.Mr.Fox let his employees have a day off.(Fox先生讓她員工休一天假。)
      <br>
      7.I foolishly let myself be tricked by the old man.(我竟然愚蠢到讓那個老人給騙了。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  使役動詞用法二 : 以名詞或形容詞作OC-->
  <el-divider/>
  <el-descriptions
      title="使役動詞用法二 : 以名詞或形容詞作OC"
      direction="vertical"
      border
  >
    <el-descriptions-item label="主詞" :span="2">S +</el-descriptions-item>
    <el-descriptions-item label="動詞 + 受詞">
      make + O + N
      <el-divider/>
      make + O + adj.
      <br>
      have + O + adj.
      <br>
      get + O + adj.
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.常用N作OC之使役動詞 : make。
      <br>
      2.常用adj作OC之使役動詞 : make,have,get。
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.His wife's unconditional love makes him a better man.(他妻子無條件的愛讓他成為一個更好的男人。)
      <br>
      2.I'll get the luggage ready in thirty minutes.(我三十分鐘內會將行李準備好。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  find/leave/keep用法-->
  <el-divider/>
  <el-descriptions
      title="find/leave/keep用法"
      direction="vertical"
      :column="4"
      border
  >
    <el-descriptions-item label="主詞">S +</el-descriptions-item>
    <el-descriptions-item label="動詞">
      find
      <br>
      leave
      <br>
      keep
    </el-descriptions-item>
    <el-descriptions-item label="受詞">
      + O +
    </el-descriptions-item>
    <el-descriptions-item>
      adj.
      <br>
      V-ing
      <br>
      p.p.
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.受詞補語判斷原則:
      <br>
      (1)接(adj.) : 補充說明受詞狀態。
      <br>
      (2)接(V-ing) : 表主動或進行，該動作為受詞所做。
      <br>
      (3)接(p.p) : 表被動或完成，受詞為該動作的接受者。
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.I found her idea creative.( 我發覺他的想法很有創意。)
      <br>
      2.Rita left her children watching TV in the living room.(Rita 留他的小孩在客廳看電視。)
      <br>
      3.Remember to keep the door locked when I am out.(我出去時記得把門鎖好。)
    </el-descriptions-item>
  </el-descriptions>
  <!--  考試-->
  <el-divider/>
  <el-descriptions
      title="Quiz Time"
      direction="vertical"
      border
  >
    <el-descriptions-item>
      <el-tag size="small">Quiz Time</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="考試題目">
      1.小丑逗趣的臉讓孩子們笑了。(make)
      <br><b style="color: blue">The clown's funny face <u>________</u>.</b>
      <br>
      2.爸爸讓技工(mechanic)檢查他的車子。(have)
      <br><b style="color: blue">Dad <u>________</u>.</b>
      <br>
      3.別讓你自己被那個花心大少(womanizer)給愚弄了。(let)
      <br><b style="color: blue">Don't <u>________</u>.</b>
      <br>
      4.當我下班時，我發覺我的車被偷了。(find)
      <br><b style="color: blue">The students <u>________</u>.</b>
      <br>
      5.別讓我一直等。(keep)
      <br><b style="color: blue">Don't <u>________</u>.</b>
      <br>
      <el-tag size="small">答</el-tag>
      1.made/makes the kids/children laugh&emsp;2.had/has the mechanic check his car&emsp;
      3.let yourself be fooled by that womanizer&emsp;4.I found my car stolen&emsp;5.keep me waiting
    </el-descriptions-item>
  </el-descriptions>
  <!--  5-->
  <el-divider/>
  <el-descriptions
      title="Point(5) S + Vt + IO + DO"
      direction="vertical"
      :column="2"
      border
  >
    <el-descriptions-item label="主詞 + 及物動詞">S + Vt +</el-descriptions-item>
    <el-descriptions-item label="IO(間接受詞)&emsp;DO(直接受詞)&emsp;prep.(介系詞)">
      IO + DO
      <br>
      DO + prep. + IO
    </el-descriptions-item>
    <el-descriptions-item>
      <el-tag size="small">說明</el-tag>
      <br>
      1.此句型中，動詞為Vt，且須接受兩個受詞，語意才完整。
      <br>
      2.直接接受Vt，稱為DO，而受該動作間接影響者(通常是人)，稱為IO。
      <br>
      3.此類接兩個受詞的Vt稱為[授與動詞]，其後的受詞排列方式有兩種:
      <br>
      (1)IO + DO。
      <br>
      (2)DO + prep. + IO。
    </el-descriptions-item>
    <el-descriptions-item label="例句">
      1.The driver showed the police officer his driver's license.( 駕駛向警察出示他的駕照。)
      <br>
      = The driver showed his driver's license to the police officer.( 駕駛向警察出示他的駕照。)
      <br><b style="color: blue">及物動詞showed之後，需接見接受詞the police officer及直接受詞his driver's license，語意才完整。</b>
      <br>
      2.Christina's parents bought her a new cell phone.(Christina的父母買給他一支手機。)
      <br>
      = Christina's parents bought a new cell phone for her.(Christina的父母買給他一支手機。)
      <br><b style="color: blue">及物動詞bought的直接動作接受者為a new cell phone，受該動作間接影響者為her，故a new cell phone為直接受詞，her為間接受詞。</b>
    </el-descriptions-item>
  </el-descriptions>
  <!--  延伸教學 : 授與動詞與介系詞搭配用法-->
  <el-divider/>
  <el-descriptions
      title="延伸教學 : 授與動詞與介系詞搭配用法"
      direction="vertical"
      :column="3"
      border
  >
    <el-descriptions-item label="主詞">S +</el-descriptions-item>
    <el-descriptions-item label="授與動詞">
      bring(帶來)&emsp;tell(告訴)&emsp;offer(提供)&emsp;hand(交給)&emsp;send(寄給)&emsp;pass(傳給)&emsp;sell(賣出)
      <br>
      show(出示)&emsp;teach(教)&emsp;give(給予)&emsp;write(寫給)&emsp;mail(寄給)&emsp;lend(借出)&emsp;pay(付給)
      <el-divider/>
      buy(買)&emsp;get(得到)&emsp;make(製作)&emsp;order(訂購)&emsp;find(找到)&emsp;leave(留給)
      <el-divider/>
      ask(詢問)
    </el-descriptions-item>
    <el-descriptions-item>
      <br>
      + DO + to + IO
      <el-divider/>
      + DO + for + IO
      <el-divider/>
      + DO + of + IO
    </el-descriptions-item>
  </el-descriptions>
  <!--  考試-->
  <el-divider/>
  <el-descriptions
      title="Quiz Time"
      direction="vertical"
      border
  >
    <el-descriptions-item>
      <el-tag size="small">Quiz Time</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="考試題目">
      1.某人寄給我一封情書。<br>
      Someone sent me a love letter.
      <br><b style="color: blue">= Someone sent <u>________</u>.</b>
      <br>
      2.Linda今早打來並留言給你。<br>
      Linda called this morning and left you a message.
      <br><b style="color: blue">= Linda called this morning and left <u>________</u>.</b>
      <br>
      3.可以請你幫忙嗎?<br>
      Can I ask you a favor?
      <br><b style="color: blue">= Can I ask <u>________</u>.</b>
      <br>
      <el-tag size="small">答</el-tag>
      1.a love letter to me&emsp;2.a message for you&emsp;3.a favor of you
    </el-descriptions-item>
  </el-descriptions>

</template>

<style scoped>

</style>