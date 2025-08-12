# flappybirds
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Flappy Bird da Shopee</title>
  <link rel="stylesheet" href="styles.css">

</head>
<body>
    <div class="background"></div>
    <img class="bird" src="img2/Woody_Woodpecker.png" alt="bird-img">
    <div class="message">
        Pressione Enter pra começar
    </div>
    <div class="score">
        <span class="score_title"></span>
        <span class="score_val"></span>
    </div>
    <script src="gfg.js"></script>
    <script src="index.js"></script>
</body>
</body>
</html>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    height: 100vh;
    width: 100vw;
}

.background {
      height: 100%;
  margin: 0;
  padding: 0;
  background-image: url(https://www.fatosdesconhecidos.com.br/wp-content/uploads/2014/07/ceu_azul_f6665cee56b20f8709940e41a85a5c43_ceu-zul-scaled.jpg); 
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.bird {
    height: 100px;
    width: 100px;
    position: fixed;
    top: 40vh;
    left: 30vw;
    z-index: 100;
}

.pipe_sprite {
    position: fixed;
    top: 40vh;
    left: 100vw;
    height: 70vh;
    width: 6vw;
    background-color: rgb(255, 255, 255);
}

.message {
    position: fixed;
    z-index: 10;
    height: 10vh;
    font-size: 10vh;
    font-weight: 100;
    color: rgb(255, 0, 0);
    top: 12vh;
    left: 20vw;
    text-align: center;
}

.score {
    position: fixed;
    z-index: 10;
    height: 10vh;
    font-size: 10vh;
    font-weight: 100;
    color: rgb(53, 66, 139);
    top: 0;
    left: 0;
}

.score_val {
    color: rgb(0, 195, 255);
}
let game_state = 'Start';
  
// Evento de pressionar tecla enter
document.addEventListener('keydown', (e) => {
  
  // Start the game if enter key is pressed
  if (e.key == 'Enter' &&
      game_state != 'Play') {
    document.querySelectorAll('.pipe_sprite')
              .forEach((e) => {
      e.remove();
    });
    bird.style.top = '40vh';
    game_state = 'Play';
    message.innerHTML = '';
    score_title.innerHTML = 'Score : ';
    score_val.innerHTML = '0';
    play();
  }
});
function play() {
  function move() {
    
    // Checar se o jogo está em andamento
    // e se não estiver, não faça nada
    if (game_state != 'Play') return;
    
    // Referencia dos obstáculos
    let pipe_sprite = document.querySelectorAll('.pipe_sprite');
    pipe_sprite.forEach((element) => {
      
      let pipe_sprite_props = element.getBoundingClientRect();
      bird_props = bird.getBoundingClientRect();
      
      // Deleta o obstáculo se ele sair da tela
    
      if (pipe_sprite_props.right <= 0) {
        element.remove();
      } else {
        // Detecta colisão entre pássaro e obstáculo
        if (
          bird_props.left < pipe_sprite_props.left +
          pipe_sprite_props.width &&
          bird_props.left +
          bird_props.width > pipe_sprite_props.left &&
          bird_props.top < pipe_sprite_props.top +
          pipe_sprite_props.height &&
          bird_props.top +
          bird_props.height > pipe_sprite_props.top
        ) {
          
          // Altera o estado do jogo para 'End'
          // e exibe a mensagem de fim de jogo caso ocorra uma colisão
    
          game_state = 'End';
          message.innerHTML = 'Press Enter To Restart';
          message.style.left = '28vw';
          return;
        } else {
          // Aumenta a pontuação se o pássaro
          // passar pelo obstáculo

          if (
            pipe_sprite_props.right < bird_props.left &&
            pipe_sprite_props.right + 
            move_speed >= bird_props.left &&
            element.increase_score == '1'
          ) {
            score_val.innerHTML = +score_val.innerHTML + 1;
          }
          element.style.left = 
            pipe_sprite_props.left - move_speed + 'px';
        }
      }
    });

    requestAnimationFrame(move);
  }
  requestAnimationFrame(move);

  let bird_dy = 0;
  function apply_gravity() {
    if (game_state != 'Play') return;
    bird_dy = bird_dy + gravity;
    document.addEventListener('keydown', (e) => {
      if (e.key == 'ArrowUp' || e.key == ' ') {
        bird_dy = -7.6;
      }
    });

    // Detecta se o pássaro colidiu

    if (bird_props.top <= 0 ||
        bird_props.bottom >= background.bottom) {
      game_state = 'End';
      message.innerHTML = 'Press Enter To Restart';
      message.style.left = '28vw';
      return;
    }
    bird.style.top = bird_props.top + bird_dy + 'px';
    bird_props = bird.getBoundingClientRect();
    requestAnimationFrame(apply_gravity);
  }
  requestAnimationFrame(apply_gravity);

  let pipe_seperation = 0;
  
  // Constant value for the gap between two pipes
  let pipe_gap = 35;
  function create_pipe() {
    if (game_state != 'Play') return;
    
    // Cria um novo elemento de obstáculo 
    // a cada 115px de movimento
    // e o adiciona ao DOM
    if (pipe_seperation > 115) {
      pipe_seperation = 0
      
      // Calcula a posição do obstáculo
      // e cria o elemento de obstáculo
      let pipe_posi = Math.floor(Math.random() * 43) + 8;
      let pipe_sprite_inv = document.createElement('div');
      pipe_sprite_inv.className = 'pipe_sprite';
      pipe_sprite_inv.style.top = pipe_posi - 70 + 'vh';
      pipe_sprite_inv.style.left = '100vw';
      
      // Adiciona o elemento de obstáculo
      // ao DOM
      document.body.appendChild(pipe_sprite_inv);
      let pipe_sprite = document.createElement('div');
      pipe_sprite.className = 'pipe_sprite';
      pipe_sprite.style.top = pipe_posi + pipe_gap + 'vh';
      pipe_sprite.style.left = '100vw';
      pipe_sprite.increase_score = '1';
      
      // Adiciona o elemento de obstáculo 
      
      document.body.appendChild(pipe_sprite);
    }
    pipe_seperation++;
    requestAnimationFrame(create_pipe);
  }
  requestAnimationFrame(create_pipe);
}
