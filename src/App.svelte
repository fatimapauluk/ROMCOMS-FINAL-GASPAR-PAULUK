<script>
  import Scroller from "@sveltejs/svelte-scroller";
  import { onMount } from "svelte";
  import Carousel from './Carousel.svelte';
  import DebugScroller from "./components/DebugScroller.svelte"


  let count;
  let index = 0; 
  let offset;
  let progress;
  let top = 0.1;
  let threshold = 0.5;
  let bottom = 0.9;

  // Contenido TITULO 
  const sections = [
    {
      title: "1. MEET CUTE",
      
    },
    {
      title: "2. EL ENAMORAMIENTO",
    
    },
    {
      title: "3. CONFLICTO",
      
    },
    {
      title: "4. REVELACIÓN",

    },
    {
      title: "5. FINAL FELIZ",
      
    },
  ];

  const meetCuteMovies = [
    { image: './images/It Happened One Night.jpg', title: 'It Happened One Night ', description: '1934', link: 'https://www.imdb.com/title/tt0025316/?ref_=nv_sr_srsg_1_tt_8_nm_0_in_0_q_it%2520happe'},
    { image: './images/The Philadelphia Story.jpg', title: 'The Philadelphia Story', description: '1940', link: 'https://www.imdb.com/title/tt0032904/?ref_=nv_sr_srsg_0_tt_7_nm_1_in_0_q_the%2520philade' },
    { image: './images/Roman Holiday.jpg', title: 'Roman Holiday', description: '1953', link: 'https://www.imdb.com/title/tt0046250/?ref_=fn_al_tt_1' },
    { image: './images/Breakfast at Tiffanys.jpg', title: 'Breakfast at Tiffany', description: '1961', link: 'https://www.imdb.com/title/tt0054698/?ref_=fn_al_tt_1' },
    { image: './images/Annie Hall.jpg', title: 'Annie Hall', description: '1977', link: 'https://www.imdb.com/title/tt0075686/?ref_=nv_sr_srsg_0_tt_5_nm_3_in_0_q_Annie%2520Hall' },
    { image: './images/Father of the Bride.jpg', title: 'Father of the Bride', description: '1950', link: 'https://www.imdb.com/title/tt0042451/?ref_=fn_al_tt_3' },
    { image: './images/Gentlemen Prefer Blondes.jpg', title: 'Gentlemen Prefer Blondes', description: '1953', link: 'https://www.imdb.com/title/tt0045810/?ref_=fn_al_tt_1' },
    { image: './images/The Graduate.jpg', title: 'The Graduate', description: '1967', link: 'https://www.imdb.com/title/tt0061722/?ref_=fn_al_tt_1' },
    { image: './images/Funny Girl.jpg', title: 'Funny Girl', description: '1968', link: 'https://www.imdb.com/title/tt0062994/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_Funny%2520Girl' },
  ];

  const enamoramientoMovies = [
    { image: './images/Notting Hill.jpg', title: 'Notting Hill', description: '1977', link: 'https://www.imdb.com/title/tt0125439/?ref_=fn_al_tt_1'  },
    { image: './images/10 Things I Hate About You.jpg', title: '10 Things I Hate About You', description: '1999', link: 'https://www.imdb.com/title/tt0147800/?ref_=nv_sr_srsg_1_tt_7_nm_0_in_0_q_10%2520Things%2520I%2520Hate%2520About%2520You' },
    { image: './images/27 Dresses.jpg', title: '27 Dresses', description: '2008', link: 'https://www.imdb.com/title/tt0988595/?ref_=nv_sr_srsg_0_tt_7_nm_1_in_0_q_27' },
    { image: './images/When Harry Met Sally.jpeg', title: 'When Harry Met Sally', description: '1989', link: 'https://www.imdb.com/title/tt0098635/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_When%2520Harry%2520Met%2520Sally'  },
    { image: './images/Pretty Woman.jpg', title: 'Pretty Woman', description: '1990', link: 'https://www.imdb.com/title/tt0100405/?ref_=fn_al_tt_1'  },
    { image: './images/Sleepless in Seattle.jpg', title: 'Sleepless in Seattle', description: '1993', link: 'https://www.imdb.com/title/tt0108160/?ref_=nv_sr_srsg_0_tt_4_nm_0_in_0_q_Sleepless%2520in%2520Seattle'  },
    { image: './images/My Best Friends Wedding.jpg', title: 'My Best Friends Wedding', description: '1997', link: 'https://www.imdb.com/title/tt0119738/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_My%2520Best%2520Friends%2520Wedding'  },
    { image: './images/Theres Something About Mary.jpg', title: 'Theres Something About Mary', description: '1998', link: 'https://www.imdb.com/title/tt0129387/?ref_=fn_al_tt_2'  },
    { image: './images/While You Were Sleeping.jpg', title: 'While You Were Sleeping', description: '1999', link: 'https://www.imdb.com/title/tt0114924/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_While%2520You%2520Were%2520Sleeping' },
    { image: './images/Bird on a Wire.jpg', title: 'Bird on a Wire', description: '1990', link: 'https://www.imdb.com/title/tt0099141/?ref_=nv_sr_srsg_0_tt_6_nm_2_in_0_q_Bird%2520on%2520a%2520Wire'  },
    { image: './images/Groundhog Day.jpg', title: 'Groundhog Day', description: '1993', link: 'https://www.imdb.com/title/tt0107048/?ref_=fn_al_tt_1'  },
    { image: './images/The Proposal.jpg', title: 'The Proposal', description: '2009', link: 'https://www.imdb.com/title/tt1041829/?ref_=fn_al_tt_1'  },
    { image: './images/What Women Want.jpg', title: 'What Women Want', description: '2000', link: 'https://www.imdb.com/title/tt0207201/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_What%2520Women%2520Want'  },
    { image: './images/Sweet Home Alabama.jpg', title: 'Sweet Home Alabama', description: '2002', link: 'https://www.imdb.com/title/tt0256415/?ref_=nv_sr_srsg_0_tt_5_nm_0_in_0_q_Sweet%2520Home%2520Alabama'  },
    

  ];

  const conflictoMovies = [
    { image: './images/Set It Up.jpeg', title: 'Set It Up', description: '2018', link: 'https://www.imdb.com/title/tt5304992/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_Set%2520It%2520Up'  },
    { image: './images/To All the Boys I’ve Loved Before.jpg', title: 'To All the Boys Ive Loved Before', description: '2018', link: 'https://www.imdb.com/title/tt3846674/?ref_=fn_al_tt_1' },
    { image: './images/La La Land.jpg', title: 'La La Land', description: '2016', link: 'https://www.imdb.com/title/tt3783958/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_La%2520La%2520Land'  },
    { image: './images/Crazy Rich Asians.jpg', title: 'Crazy Rich Asians', description: '2018', link: 'https://www.imdb.com/title/tt3104988/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_Crazy%2520Rich%2520Asians'  },
    { image: './images/The Kissing Booth.png', title: 'The Kissing Booth', description: '2018', link: 'https://www.imdb.com/title/tt3799232/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_The%2520Kissing%2520Booth'  },
    { image: './images/Culpa mia.jpg', title: 'Culpa mia', description: '2023', link: 'https://www.imdb.com/title/tt21909764/?ref_=nv_sr_srsg_0_tt_7_nm_1_in_0_q_Culpa%2520mia' },
    { image: './images/The idea of You.jpg', title: 'The idea of You', description: '2024', link: 'https://www.imdb.com/title/tt9466114/?ref_=fn_al_tt_1'},
    { image: './images/Upgraded.jpg', title: 'Upgraded', description: '2024', link: 'https://www.imdb.com/title/tt21830902/?ref_=nv_sr_srsg_4_tt_8_nm_0_in_0_q_Upgrade' },
    { image: './images/A traves de mi ventana.jpg', title: 'A traves de mi ventana', description: '2022', link: 'https://www.imdb.com/title/tt14463484/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_A%2520traves%2520de%2520mi%2520venta' },

  ];

  const conflictoSeries = [
    { image: './images/Un cuento perfecto.jpg', title: 'Un cuento perfecto', description: '2018', link: 'https://www.imdb.com/title/tt21909764/?ref_=nv_sr_srsg_0_tt_7_nm_1_in_0_q_Culpa%2520mia' },
    { image: './images/Maxton Hall.jpg', title: 'Maxton Hall', description: '2018', link: 'https://www.imdb.com/title/tt27792190/?ref_=nv_sr_srsg_0_tt_4_nm_4_in_0_q_Maxton%2520Hall' },
    { image: './images/Mi vida con los chicos Walter.jpg', title: 'Mi vida con los chicos Walter', description: '2023', link: 'https://www.imdb.com/title/tt8323628/?ref_=fn_al_tt_1' },
    { image: './images/El verano que me enamore.jpg', title: 'The summer I turned pretty', description: '2023', link: 'https://www.imdb.com/title/tt14016500/?ref_=fn_al_tt_1' },
    { image: './images/Nobody Wants This.jpg', title: 'Nobody Wants This', description: '2024', link: 'https://www.imdb.com/title/tt26933824/?ref_=nv_sr_srsg_2_tt_8_nm_0_in_0_q_Nadie' },
    { image: './images/Heartstopper.jpg', title: 'Heartstopper', description: '2023', link: 'https://www.imdb.com/title/tt10638036/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_Heartstopper' },
    { image: './images/XO, Kitty.jpg', title: 'XO, Kitty', description: '2023', link: 'https://www.imdb.com/title/tt14490706/?ref_=nv_sr_srsg_0_tt_3_nm_5_in_0_q_XO%252C%2520Kitty' },
  ];
 
</script>



<header> 
  <img src="./images/titulo.svg" alt="Imagen Header">
  <div class="flecha">
    <img src="./images/Group 67.svg" alt="flecha">
  </div>  <div class="description-box">
    <p class="bajada">La comedia romántica (también conocida como <strong>rom-com</strong>) es un subgénero de comedia y ficción romántica, que se centra en líneas argumentales alegres y humorísticas centradas en ideas románticas, de cómo el amor verdadero es capaz de superar todos los obstáculos.</p>
  </div>

  <h2>ESTRUCTURA DE LAS ROM-COMS</h2>

  <div class="timeline-container">
    <div class="timeline">
      <div class="oval">Meet Cute</div>
      <div class="line"></div>
      <div class="oval">Enamoramiento</div>
      <div class="line"></div>
      <div class="oval">Conflicto</div>
      <div class="line"></div>
      <div class="oval">Revelación</div>
      <div class="line"></div>
      <div class="oval">Final Feliz</div>
    </div>
  </div>
</header>

<main>

 

  <!-- Contenedor fijo  -->

<!-- Contenedor fijo  -->

<div class="content-container">
  <div class="main-title">
    <h1>{sections[index]?.title}</h1>
  </div>
  <div class="columns-container">
    <div class="column">
      <h2 class="section-title">EN LAS PELÍCULAS</h2>
      
    </div>
<!-- Divider con el corazón -->
<div class="vertical-dividerfix">
<div class="heart-container">
  <svg
    class="heart"
    width="66"
    height="56"
    viewBox="0 0 66 56"
    xmlns="http://www.w3.org/2000/svg"
  >
    <!-- Contorno del corazón -->
    <path
      d="M47.295 44.2045C42.8184 48.0127 37.784 51.5719 33.0145 55C28.2365 51.5672 23.191 48.0006 18.705 44.1848C8.46181 35.4727 -6.27201 20.883 5.13509 7.05359C12.0935 -1.38226 23.2068 -0.494571 30.7864 6.5854C31.5838 7.32982 32.2544 8.18286 33.0098 8.96287C33.7578 8.18755 34.4237 7.34293 35.2136 6.60506C42.7932 -0.474907 53.9065 -1.36166 60.8649 7.07419C72.272 20.9036 57.5382 35.4933 47.295 44.2045Z"
      stroke="black"
      fill="none"
      stroke-miterlimit="10"
      stroke-linejoin="round"
    ></path>

    
    <defs>
      <!-- Gradiente para el relleno -->
      <linearGradient id="gradient" x1="0" y1="1" x2="0" y2="0">
        <stop offset="0%" stop-color="#fda1b1" />
        <stop offset="100%" stop-color="#fda1b1" />
      </linearGradient>

      <!-- Máscara de recorte -->
      <clipPath id="clip">
        <rect
          x="0"
          y="{56 - 56 * progress}"
          width="66"
          height="{56 * progress}"
        />
      </clipPath>
    </defs>

    <!-- Capa rellena -->
    <path
      d="M47.295 44.2045C42.8184 48.0127 37.784 51.5719 33.0145 55C28.2365 51.5672 23.191 48.0006 18.705 44.1848C8.46181 35.4727 -6.27201 20.883 5.13509 7.05359C12.0935 -1.38226 23.2068 -0.494571 30.7864 6.5854C31.5838 7.32982 32.2544 8.18286 33.0098 8.96287C33.7578 8.18755 34.4237 7.34293 35.2136 6.60506C42.7932 -0.474907 53.9065 -1.36166 60.8649 7.07419C72.272 20.9036 57.5382 35.4933 47.295 44.2045Z"
      fill="url(#gradient)"
      clip-path="url(#clip)"
    ></path>
  </svg>
</div>
</div>


    <div class="debug_scroller">
      <p class="p_debug">
      </p>
    </div>



    <div class="column">
      <h2 class="section-title">EN LA HISTORIA</h2>
      
    </div>
  </div>
</div>  



  <!-- Scroller para contenido dinámico -->
  <Scroller
    top={top}
    threshold={threshold}
    bottom={bottom}
    bind:count={count}
    bind:index={index}
    bind:offset={offset}
    bind:progress={progress}
  >
    <div slot="foreground" class="foreground_container">
      

     <!-- Meet Cute --> 
        <section class="step_foreground" >

                  <!-- GIF --> 
        <div class="gif">
          <img src="./images/meet cute.gif" alt="Gif romántico">
          <p>Meet - cute de "It Happened One Night"</p>
        </div>

          <div class="columns-container">

            <!-- COLUMNA izquierda --> 
            <div class="column">
              <p class="description">
                Meet cute es una expresión que se refiere, en el cine y la TV, a 
                una escena en la que las <span class="highlight-green">dos</span> personas que formarán una futura 
                pareja romántica <span class="highlight-green">se encuentran por primera vez</span>, generalmente 
                en circunstancias inusuales, divertidas o lindas.
              </p>


              <div class="graficotorta">
                <div class="cadagrafico">
                  <img src="./images/prendas mujeres.png" alt="">
                  <p><strong>Vestimenta</strong> de las <strong>mujeres</strong> en las meet-cute [1]</p>
                </div>
                <div class="cadagrafico">
                  <img src="./images/prendas hombres.png" alt="">
                  <p><strong>Vestimenta</strong> de los <strong>hombres</strong> en las meet-cute[1]</p>
                </div>
              </div>

              <div class="graficolargo">
                <img src="./images/Lugares.meetcutes.png" alt="">
                <p><strong>Lugares</strong> en los que las meet-cutes suelen suceder [1]. </p>
               </div>

              </div>
        <div class="vertical-divider"></div>

<!-- COLUMNA DERECHA --> 
<div class="column">
  <p class="description">
    El primer encuentro entre el público y las rom-coms surge en los 
    <span class="highlight-green">años 30 y 40</span> en Hollywood. Este género surgió a partir de:
  </p>
  <div class="categories-container">
    <div class="category">
      <h3 class="pink-title">GÉNEROS:</h3>
      <ul>
        <li>Comedia</li>
        <li>Romance</li>
        <li>Dramaturgia romántica</li>
        <li>Screwball</li>
      </ul>
    </div>
<div class="category-mas">
<img src="./images/signomasmeet.svg" alt="">
</div>
    <div class="category">
      <h3 class="pink-title">CONTEXTO:</h3>
      <ul>
        <li>Pos - guerra</li>
        <li>American dream</li>
      </ul>
    </div>
  </div>

  <div class="graficolargo">
    <img src="./images/cant.romcoms.png" alt="">
    <p><strong>Crecimiento</strong> de las rom-coms en este período [2].</p>
  </div>



</div>



</div>


<p class="carruseltexto"> Peliculas</p>
<Carousel items={meetCuteMovies} />


<div class="graficolargo">
<img src="./images/encuentro casual.svg" alt="">
</div>
        </section>
      
<!-- Enamoramiento--> 
<section class="step_foreground" >

  <div class="gif">
    <img src="./images/EL ENAMORAMIENTO.gif" alt="Gif romántico">
    <p>Enamoramiento de "When Harry Met Sally"</p>
  </div>

  <div class="columns-container">

<!-- COLUMNA IZQ --> 
    <div class="column">
      <p class="description">
        El enamoramiento suele mostrarse a través de un "montage", una secuencia de <span class="highlight-green">momentos felices</span>. Este recurso permite resumir la evolución de la relación y transmitir al espectador la <span class="highlight-green">magia y emoción</span> de su vínculo en pocos minutos. Las situaciones que más frecuentemente aparecen en los montage son:
      </p>

      <div class="category">
        <ul>
          <li>Riendo en la cama</li>
          <li>Peleando de forma juguetona</li>
          <li>Preparando comida juntos</li>
          <li>Caminando por el parque o playa en un día soleado o al atardecer</li>
          <li>Disfrutando actividades espontáneas 
            (paseos en bicicleta o visitas a ferias) </li>
      


        </ul>
      </div>


        </div>

        <div class="vertical-divider"></div>
<!-- COLUMNA DER --> 
        <div class="column">
          <p class="description">
            El boom de las comedias románticas se dio entre los <span class="highlight-green">años 80 y 2010</span>, marcando una <span class="highlight-green">época dorada</span> en la que este género se consolidó como favorito en el cine. Durante esta época, surgieron muchos <span class="highlight-green">actores famosos</span> a partir de comedias románticas. Algunos de ellos son:
          </p>
            

            <div class="gallery">
              <!-- Actor 1 -->
              <div class="actor-card">
                <img src="./images/Julia Roberts.png" alt="Julia Roberts" class="actor-image">
                <p class="actor-name">Julia Roberts</p>
                <div class="awards">
                  <img src="./images/oscar.png" alt="Oscar" class="award-icon">
                  <img src="./images/oscar.png" alt="Oscar" class="award-icon">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                </div>
              </div>
              
              <!-- Actor 2 -->
              <div class="actor-card">
                <img src="./images/Reese Witherspoon.png" alt="Reese Witherspoon" class="actor-image">
                <p class="actor-name">Reese Witherspoon</p>
                <div class="awards">
                  <img src="./images/oscar.png" alt="Oscar" class="award-icon">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">

                </div>
              </div>
            
              <!-- Actor 3 -->
              <div class="actor-card">
                <img src="./images/Sandra Bullock.png" alt="Sandra Bullock" class="actor-image">
                <p class="actor-name">Sandra Bullock</p>
                <div class="awards">
                  <img src="./images/oscar.png" alt="Oscar" class="award-icon">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                </div>
              </div>
            
              <!-- Actor 4 -->
              <div class="actor-card">
                <img src="./images/Matthew McConaughey.png" alt="Matthew McConaughey" class="actor-image">
                <p class="actor-name">Matthew McConaughey</p>
                <div class="awards">
                  <img src="./images/oscar.png" alt="Oscar" class="award-icon">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                </div>
              </div>
              
              <!-- Actor 5 -->
              <div class="actor-card">
                <img src="./images/Hugh Grant.png" alt="Hugh Grant" class="actor-image">
                <p class="actor-name">Hugh Grant</p>
                <div class="awards">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                </div>
              </div>

              <!-- Actor 6 -->
              <div class="actor-card">
                <img src="./images/Kate Hudson.png" alt="Kate Hudson" class="actor-image">
                <p class="actor-name">Kate Hudson</p>
                <div class="awards">
                  <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                </div>
              </div>
              
            </div>

            <!-- Legend -->
            <div class="legend">
              <div class="legend-item">
                <img src="./images/oscar.png" alt="Oscar" class="legend-icon">
                <span>Premio Oscar ganado</span>
              </div>
              <div class="legend-item">
                <img src="./images/Globo de Oro.png" alt="Globo de Oro" class="award-icon">
                <span>Premio Globo de Oro ganado</span>
              </div>
            </div>
            
          </div>
        </div>



      <p class="carruseltexto"> Peliculas</p>
      <Carousel items={enamoramientoMovies} />

      <div class="graficolargo">
        <img src="./images/nada es tan facil.svg" alt="">
       </div>

        </section>
        
     
<!-- Conflicto--> 
<section class="step_foreground" >

  <!-- GIF --> 

        <div class="gif">
          <img src="./images/conflicto.gif" alt="Gif romántico">
          <p>Conflicto de "Notting Hill"</p>
        </div>

  <div class="columns-container">

<!-- COLUMNA IZQ --> 
    <div class="column">
      <p class="description">
        El conflicto es el momento que pone a prueba la relación entre los protagonistas, <span class="highlight-green">interrumpiendo la fase de enamoramiento</span>. A menudo, el conflicto lleva a una <span class="highlight-green">ruptura temporal </span>o a un <span class="highlight-green">distanciamiento</span>, que obliga a los protagonistas a reflexionar sobre sus sentimientos y lo que realmente desean.
      </p>

      <div class="graficolargo">
        <img src="./images/Clichés conflicto.png" alt="">
        <p>Algunos de los <strong>clichés que pueden resultar en conflicto </strong>[3].</p>
      </div>

      <div class="graficolargo">
        <iframe src='https://flo.uri.sh/visualisation/20279307/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:600px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe><div style='width:100%!;margin-top:4px!important;text-align:right!important;'><a class='flourish-credit' href='https://public.flourish.studio/visualisation/20279307/?utm_source=embed&utm_campaign=visualisation/20279307' target='_top' style='text-decoration:none!important'> </a></div>        
        <p><strong>Problemas típicos</strong> de las comedias románticas [3].</p>
      </div>

      <div class="graficolargo">
        <img src="./images/triangulo amoroso.png" alt="">
        <p>Cantidad de veces que aparecen <strong>triángulos y cuadrángulos amorosos</strong> en 70 rom-coms analizadas [1].</p>
      </div>

        </div>

        <div class="vertical-divider"></div>
<!-- COLUMNA DER --> 
        <div class="column">
          <p class="description">
            A partir del fin de los 2000s, la producción de romcoms fue disminuyendo. Se genera una crisis en el género debido a diferentes factores <span class="highlight-green">sociales y tecnicos</span> que ocurrieron con la evolución del tiempo.
          </p>

          <div class="graficolargo">
            <iframe src='https://flo.uri.sh/visualisation/20267698/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:300px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe><div style='width:100%!;margin-top:4px!important;text-align:right!important;'><a class='flourish-credit' href='https://public.flourish.studio/visualisation/20267698/?utm_source=embed&utm_campaign=visualisation/20267698' target='_top' style='text-decoration:none!important'></a></div>            
            <p>Este gráfico muestra que la gente a la que <strong>no le gustan las rom-coms</strong>, suele pensar que son <strong>menos realistas</strong> [4].</p>
          </div>
    

          <p class="description">La experiencia de <span class="highlight-rosa">salir ha cambiado mucho, pero las rom-coms se mantienen en relatos tradicionales</span>. Hoy, cada vez más personas se conocen en redes y apps de citas, lo que se aleja de las situaciones típicas de las rom-coms, haciendo que muchos se sientan menos identificados con este género.</p>

          <div class="graficolargo">
            <img src="./images/cant. romcoms 2.png" alt="">
            <p>Este gráfico muestra que la <strong>cantidad de rom-coms ha disminuido</strong> en los últimos años [5].</p>
          </div>
    

          <p class="description">Las plataformas de streaming contribuyeron al declive de las comedias románticas, ya que los estudios de cine <span class="highlight-rosa">redujeron la producción de películas de presupuesto medio</span> al no poder competir con el volumen de contenido de las plataformas. Los estudios optaron por concentrarse en grandes producciones, siguiendo una estrategia de “go big or go home”.</p>

          <p class="description">Además, las comedias románticas dejaron de ser un género del cual nacen actores famosos. Kate Hudson comenta que es cada vez más <span class="highlight-rosa">difícil conseguir actores masculinos</span> que quieran participar de comedias románticas.</p>


        
            </div>



        </div>


        <p class="carruseltexto"> Peliculas</p>
        <Carousel items={conflictoMovies} />

        <p class="carruseltexto"> Series</p>
        <Carousel items={conflictoSeries} />


        <div class="graficolargo">
          <img src="./images/emociones son fuertes.svg" alt="">
         </div>
  
        </section>

<!-- Revelacion--> 


<section class="step_foreground revelacion">

    <!-- GIF -->
    <div class="gif">
      <img src="./images/el gran gesto.gif" alt="Gif romántico">
      <p>El gran gesto de "Anyone but you"</p>
    </div>
  
  <div class="columns-container">
    <!-- COLUMNA IZQ -->
    <div class="column">
      <p class="description">
        Este momento ocurre cuando <span class="highlight-green">uno de los protagonistas comprende que quiere estar con la otra persona</span>, marcando un punto clave tras el conflicto. Este instante de claridad va acompañado de una gran muestra de amor o "grand gesture" que busca reconciliar a la pareja.
      </p>

      <div class="graficolargo">
        <img src="./images/Gran Gesto.png" alt="">
        <p>Cantidad de veces en las que se presenta un <strong>“gran gesto”</strong> [1].</p>
      </div>

      <p class="description">Estos son algunos de los “grandes gestos” más típicos:</p>
      <div class="category">
        <ul>
          <li>Declaración pública de amor</li>
          <li>Perseguir a la pareja en aeropuertos, estaciones o calles</li>
          <li>Regalo simbólico</li>
          <li>Sorpresa romántica (ej: decorar un lugar importante)</li>
          <li>Serenatas o canciones dedicadas</li>
        </ul>
      </div>
    </div>

    <div class="vertical-divider"></div>

    <!-- COLUMNA DER -->
    <div class="column">
      <div class="container-revelacion">
        <div class="izquierda-revelacion">
       
  <div class="izquierda-revelacion-texto">
    
        <p>
            En 2023, se estrenó en los cines la muy exitosa rom-com
            <span class="highlight-green">"Anyone but you"</span> que marca un posible punto de inflexión
            en la historia de las rom-coms.
        </p>
  
        <p class="description-monto">Recaudación en todo el mundo:</p>
        <div class="monto">220.297.199 US$</div>
  
      </div>
  
        <div class="movie-revelacion">
          <img src="./images/Anyone but you.jpg" alt="">
        </div>
  
        
       </div>
  
        <p class="description">Su gran éxito se debió a 2 principales factores:</p>
        <div class="category">
          <ul>
            <li>Marketing en redes sociales</li>
            <li>Storytelling que respeta la estructura de las rom-coms e incorpora elementos modernos</li>
          </ul>
        </div>
        <div class="description-revelacion">
          <p>Además, se empezaron a hacer muchas comedias románticas en <span class="highlight-green">formato de serie</span>. Muchos argumentan que es un formato que satisface más los requerimientos del público actual ya que permite desarrollar más la relación de la pareja y sus conflictos lo que le otorga un mayor grado de realismo y credibilidad.</p>
        </div>
  
    </div>
  </div>
  </div>
  
    <div class="graficolargo">
      <img src="./images/siempre vuelven.svg" alt="">
     </div>

</section>


<!-- Final feliz--> 
<section class="step_foreground" >

  <!-- GIF --> 


  <div class="gif">
    <img src="./images/happyend.gif" alt="Gif romántico">
    <p>Final Feliz de "10 Things I Hate About You"</p>
  </div>

  <div class="columns-container">

<!-- COLUMNA IZQ --> 
    <div class="column">
      <p class="description">
        El final feliz en las comedias románticas es el momento en que la pareja, <span class="highlight-green">tras superar conflictos</span>, se une de forma definitiva. Este cierre celebra la conexión entre los protagonistas y da una sensación de optimismo y satisfacción al público, reforzando la idea de que el amor puede superar cualquier obstáculo.
      </p>

      <div class="graficotorta">
        <div class="cadagrafico">
          <img src="./images/Final feliz.png" alt="">
          <p>Porcentaje en el que hay un <strong>final feliz</strong> en las rom-com [1].</p>
        </div>
        <div class="cadagrafico">
          <img src="./images/beso final.png" alt="">
          <p>Porcentaje en el que la película <strong>termina con un beso</strong> [1].</p>
        </div>
      </div>

        </div>

        <div class="vertical-divider"></div>
<!-- COLUMNA DER --> 
        <div class="column">
          <p class="description">
            "Anyone But You" demostró que las comedias románticas clásicas de presupuesto medio aún pueden triunfar en el cine contemporáneo, demostrando que una historia genuina y personajes auténticos siguen siendo atractivos para el público.  </p>

            <p class="description">Además, el éxito creciente de las rom-coms en formato de serie sugiere que este género sigue teniendo potencial. </p>


              <p class="description">Sin embargo, ¿será esto suficiente para revitalizar el género? </p>

 
                <p class="description"><span class="highlight-green"><strong>¿Habrá un final feliz para las romcoms?</strong></span></p>

         
            </div>



        </div>


  </section>


    </div>

  </Scroller>







</main>

  <!-- PARTE MAS DATOS --> 
<div class="final">


  <div class="final-titulos">
  <img src="./images/finaldatos.svg" alt="">
  <p><a href="https://docs.google.com/spreadsheets/d/13x-t2HQgn5rd-uNTbVbDJsrGwrqS7-qwj0ILt0LiltI/edit?gid=0#gid=0" target="_blank" rel="comedias romanticas excel">FUENTE DE INVESTIGACIÓN PROPIA </a></p>


  <div class="container">
    <!-- Sección Nacionalidad -->
    <div class="section">
        <h3>NACIONALIDAD</h3>
        <p class="descripciongrafico"> Porcentaje de rom-coms estadounidenses y de otras nacionalidades</p> 
        <img src="./images/WAFFLE.png" alt="Nacionalidad">

        
        <p class="descripcionlarga">
          La mayoría de las rom-coms provienen de <span class="highlight-green">Estados Unidos</span> debido a la influencia de <span class="highlight-green">Hollywood</span>, una industria poderosa que popularizó el género y su fórmula rentable. Esto ha incentivado su inversión y exportación para captar audiencias globales.
        </p>
    </div>

   

    <!-- Sección Dirección -->
    <div class="section2">
        <h3>DIRECCIÓN</h3>
        <p class="descripciongrafico"> Porcentaje de mujeres y hombres que dirigen rom-coms</p> 
        <img src="./images/DIRECTORES.png" alt="DIRECTORES">
        <p class="descripcionlarga">
          La <span class="highlight-green"> disparidad de género</span> en la dirección refleja un desequilibrio en la industria cinematográfica. Aunque las rom-coms apuntan a una audiencia femenina, las directoras han tenido menos acceso por el prejuicio de que los hombres son “más comerciales”. Aun así, ahora se ven avances hacia una mayor inclusión.
        </p>
    </div>


</div>
<div class="tabla">
  <iframe src='https://flo.uri.sh/visualisation/20513477/embed' title='Interactive or visual content' class='flourish-embed-iframe' frameborder='0' scrolling='no' style='width:100%;height:850px;' sandbox='allow-same-origin allow-forms allow-scripts allow-downloads allow-popups allow-popups-to-escape-sandbox allow-top-navigation-by-user-activation'></iframe><div style='width:100%!;margin-top:4px!important;text-align:right!important;'><a class='flourish-credit' href='https://public.flourish.studio/visualisation/20513477/?utm_source=embed&utm_campaign=visualisation/20513477' target='_top' style='text-decoration:none!important'> </a></div>
</div>

</div>
  


<div class="containerend">
  <div class="lineend"></div>
  <img src="./images/theend.svg" alt="Gif romántico">
</div>

</div>



<footer>
  <div class="footer-top">
    <span>Visualización de datos, UTDT</span>
    <span>@paulukfatimalab @labdezoe</span>
  </div>

  <div class="content">
    <p>Publicado: 30 / 11 / 2024 <br> <br> <strong>Fuentes:</strong> <br> 

      [ 1 ]: the, I. (2018, March 5). Is the Rom Com Dead? Breaking Down 79 Romantic Comedies | Vanity Fair. YouTube. https://www.youtube.com/watch?v=SgyaTrDDY4k <br>
      ‌[ 2 ]: to, C. (2009, August 24). Wikimedia list article. Wikipedia.org; Wikimedia Foundation, Inc. https://en.wikipedia.org/wiki/List_of_romantic_comedy_films <br>
      ‌[ 3 ]: Dodds, L. (2021). Revealed: The Top Clichés of 100 Rom Coms. Mattress Online. https://www.mattressonline.co.uk/blog/sleep-news/rom-com-cliches/ <br>
      [ 4 ]: Orth, T. (2023, February 2). Americans’ relationship with romantic comedies: It’s complicated. Yougov.com; YouGov. https://today.yougov.com/entertainment/articles/45146-americans-relationship-romantic-comedies-poll <br>
      ‌[ 5 ]: Hart, S. (2023, February 14). Is the romantic comedy ready for a comeback? Reuters. https://www.reuters.com/graphics/USA-FILM/akveqmlarvr/ <br>
      IMDb: Puntuaciones, reseñas y dónde ver las mejores películas y programas de televisión. (2024). IMDb. https://www.imdb.com/</p>

</footer>


<style>
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&family=Roboto+Flex:opsz,wght@8..144,100..1000&display=swap');

:global(body) {
  font-family: "Roboto Flex", sans-serif;
  background-color: #FAFAF9;
  }


  main {
    background-color: #FAFAF9;
    margin-left: 80px;
    margin-right: 80px;
            
}


header {
    text-align: center;
    padding: 40px 20px;
    margin-bottom: 100px;

  }

  .flecha {
  display: flex;
  justify-content: center; 
  align-items: center; 
  margin-top: 150px;
}

  .description-box {

    max-width: 800px;
    margin: 30px auto;
    margin-bottom: 150px;
  }

  .bajada {
    font-size: 24px;
    color: #383838;
    margin-top: 120px;   
    line-height: 1.5;
  }

  h2 {
    color: #383838;
    font-weight: bold;
    font-size: 24px;
    margin: 0px 0 20px;
  }

  .timeline-container {
    padding: 20px;
  }

  .timeline {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0;
  }

  .oval {
    background-color: #ffE4E8;
    width: 170px;  
    height: 50px;  
    border-radius: 70%;  
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 18px;
    color: #383838;
    white-space: nowrap;
    z-index: 1;
    border: 1px dashed #383838;  /* Borde suave en rosa */
  }
  .line {
    width: 50px;
    height: 1px;
    background-color: #383838;
  }

  .section-title {
  font-size: 20px;
  padding-bottom: 5px;
  border-bottom: 2px solid #FFB6C1;
  width: 100%;
  text-align: center;
  display: block;
}

  .content-container {
    position: sticky;
    top: 0;
    background-color: #FAFAF9;
    z-index: 10;
    
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .columns-container {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    width: 100%;
  
  }

  .column {
    flex: 1;
    text-align: center;
  }

  .vertical-divider {
    width: 1px;
    background-color: #383838;
    align-self: stretch;
    margin-left: 80px;
    margin-right: 80px;
    
  }

  .main-title {
    text-align: center;
  
  }

  .main-title h1 {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    background-color: #ffE4E8;
    width: 370px;
    height: 55px;
    border-radius: 70%;
    font-size: 28px;
    color: #383838;
    white-space: nowrap;
    font-weight: 300;
    border: 1px dashed #383838;
  }

.description {
  font-size: 18px;
  line-height: 1.6;

  text-align: left;
  width: 100%;
}



  .highlight-green {
    color: #A4BE7B;
  }
  .highlight-rosa {
    color: #FDA1B1;
  }







.pink-title {
  color: #FFB6C1;
  margin-bottom: 15px;
  font-size: 18px;
  text-align: left;  
}


.category ul {
  list-style: disc;  
  padding-left: 20px;  
  margin: 0;
}

.category li {
  margin-bottom: 10px;
  font-size: 18px;
  text-align: left;  
  color: #383838;
}





  /* Responsive adjustments */
  @media (max-width: 768px) {
    .content-container {
      padding: 20px;
    }
    .columns-container {
      flex-direction: column;
    }
    .vertical-divider {
      display: none;
    }
  }

 
  .step_foreground {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  height: auto; /* ACA CAMBIAR EL LARGO DE CADA STEP */
  margin-bottom: 20px;
}
  
/* Estilos para el GIF */
.gif {
  display: inline-block; 
  border: 2px dashed #383838; 
  padding: 10px; 
  text-align: center; 
  margin-bottom: 50px;
  border-radius: 5px;
}

.gif img {
  width: 550px; 
  height: auto; 
  display: block; 
  margin-bottom: 10px; 
  border-radius: 5px;

}

.gif p {
  font-size: 16px; 
  color: #383838; 
  margin: 0; 
}


/* DATOS FINAL */

.final-titulos{
  text-align: center;
  justify-content: center;
  margin-top: 500px;
}


/* Estilo para el contenedor de gráficos de torta */
.graficotorta {
  display: flex;
  justify-content: space-between; 
  gap: 20px; 
  width: 100%; 
  max-width: 600px; 
  margin: 20px auto; 
  margin-top: 0px;
  margin-bottom: 50px;

}

/* Estilo individual para cada gráfico */
.cadagrafico img {
  width: 100%; 
  height: auto; 
  max-width: 300px; 
  display: block; 
  margin: 0 auto; 
}

.graficolargo {
  margin-top: 50px;
  margin-bottom: 70px;

}


/* Estilo para el gráfico largo */
.graficolargo img {
  width: 100%; 
  max-width: 600px; 
  height: auto; 
  display: block;
}

/* General layout */

.gallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 170px;
  gap: 0px;
  margin-top: 40px;
}

/* Actor card */
.actor-card {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.actor-image {
  width: 70px;
  height: 70px;
  object-fit: cover;
}

.actor-name {
  margin: 10px 0 5px;
  font-weight: 400;
  font-size: 18px;
  color: #383838;
  margin-bottom: 8px;
}

.awards {
  display: flex;
  gap: 2px;
}

.award-icon {
  width: 20px;
  height: 30px;
}

/* Legend */
.legend {
  margin-top: 30px;
  display: flex;
  justify-content: center;
  gap: 20px;
  font-size: 18px;
  color: #383838;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 8px;
}

.legend-icon {
  width: 20px;
  height: 30px;
}

.final-titulos{
  text-align: center;
  justify-content: center;
}

.container {
        display: flex;
        justify-content: space-around;
        align-items: flex-start;
        gap: 10px;
        padding: 2rem;
        
    }

    .section {
        flex: 1;
        max-width: 600px;
        text-align: center;
    }

    .section img {
        width: 100%;
        height: auto;
        
    }

    .section2 {
        flex: 1;
        max-width: 600px;
        text-align: center;
    }

    .section2 img {
        width: 300px;
        height: auto;
        
    }

    .section h3 {
        margin: 1rem 0 0.5rem;
        font-size: 24px;
        color: #000000;
    }

    .descripcionlarga {
        font-size: 18px;
        line-height: 1.5;
        color: #000000;
        width: 100%;
  text-align: left;
    }


    .containerend {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f9f9f9; 
    margin: 0;
    
  }

.containerend img {
  height: 355px;

}

  .lineend {
    width: 1px;
    height: 80px; 
    background-color: #000000; 
    margin-bottom: 20px; 
  }



.descripciongrafico{
  text-align: center;
  width: 100%;
  justify-content: center;
  font-size: 18px;
}


/*carrusel*/
.carruseltexto {
  width: 100%;
  text-align: left;
  font-size: 28px; 
  margin-top: 50px;
  font-weight: bold;
}

  /*corazon*/

.vertical-dividerfix {
  position: relative;
  width: 1px;
 
  align-self: stretch;
  margin: 0 80px;
}

.heart-container {
  position: absolute;
  top: 50%; 
  left: 50%;
  transform: translate(-50%, -50%); 
  display: flex;
  justify-content: center;
  align-items: center;
}

.heart {
  width: 56px;
  height: 46px;
}

.debug_scroller {
  position: fixed;
  bottom: 10px;
  left: 10px;
}

.p_debug {
  font-size: 0.8em;
  margin: 0;
  padding: 0;
}

.category li::marker {
  color: #A5B82E; 
}


  .footer-top {
    background-color: #ffe4e6; 
    padding: 1.5rem;
    margin-top: 2rem;

    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;

    border-top: 2px dashed black;
    border-right: 2px dashed black;
    border-bottom: none;
    border-left: 2px dashed black;
    display: flex;
    justify-content: space-between;
    font-weight: bold;
  }

  .content {
    background-color: #FAFAF9; 
    border: 2px dashed black;
    padding: 30px;
    display: flex;
    justify-content: space-between;
    font-size: 14px;
    line-height: 22px;
  }


  .container-revelacion {
  font-size: 18px;
  line-height: 1.6;

  text-align: left;
  width: 100%;
     

    }
    .movie-revelacion {
        display: flex;
        justify-content: center;
        margin-left: 10px;
    }
    .movie-revelacion img {
       width: 153px;
        height: 227px;
        border-radius: 8px;
        
    }
    .monto {
        text-align: left;
        font-size: 30px;
        font-weight: light;
    
    }
  
    .description-revelacion {
        margin-top: 2rem;
        line-height: 1.6;
    }


    .izquierda-revelacion{
      display: flex;
      gap: 10px;
    }

    .description-monto{
      font-size: 18px;

      text-align: left;
      width: 100%;
    }

    .category-mas{
      align-items: center;
    }

    .categories-container {
      display: flex;

      gap: 70px;  
      width: 100%;
      margin-top: 20px;
      margin-bottom: 20px;
    }
    
</style>