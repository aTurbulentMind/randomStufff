
	<script>
		const cards = document.querySelectorAll('.card');
		
		const toggleExpansion = (element, to, duration = 350) => {
		  return new Promise((res) => {
		    element.animate([
		      {
			top: to.top,
			left: to.left,
			width: to.width,
			height: to.height
		      }
		    ], {duration, fill: 'forwards', ease: 'ease-in'})
		    setTimeout(res, duration);
		  })
		}

		const fadeContent = (element, opacity, duration = 300) => {
			return new Promise(res => {
				[...element.children].forEach((child) => {
					requestAnimationFrame(() => {
						child.style.transition = `opacity ${duration}ms linear`;
						child.style.opacity = opacity;
					});
				})
				setTimeout(res, duration);
			})
		}

		const getCardContent = (title, type) => {
			return `
				<div class="card-content">
					<h2>${title}</h2>
					<img src="./assets/${type}.png" alt="${title}">
					<p>
						Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolor eum ipsa molestiae nesciunt nostrum porro
						reprehenderit? Animi corporis deleniti dolore laborum, nemo pariatur temporibus voluptatem.
					</p>
					<p>
						Lorem ipsum dolor sit amet, consectetur adipisicing elit. Atque eligendi fuga ullam? Aperiam blanditiis
						cupiditate dicta eius exercitationem explicabo fugit, impedit iure libero nam nihil nisi perferendis
						provident quaerat repellendus vitae voluptate? Aliquid amet architecto asperiores aut consequuntur
						corporis debitis ea eveniet in maiores, nam placeat quae, ratione rerum ullam?
					</p>
					<p>
						Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolor eum ipsa molestiae nesciunt nostrum porro
						reprehenderit? Animi corporis deleniti dolore laborum, nemo pariatur temporibus voluptatem.
					</p>
					<p>
						Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolor eum ipsa molestiae nesciunt nostrum porro
						reprehenderit? Animi corporis deleniti dolore laborum, nemo pariatur temporibus voluptatem.
					</p>
					<p>
						Lorem ipsum dolor sit amet, consectetur adipisicing elit. Atque eligendi fuga ullam? Aperiam blanditiis
						cupiditate dicta eius exercitationem explicabo fugit, impedit iure libero nam nihil nisi perferendis
						provident quaerat repellendus vitae voluptate? Aliquid amet architecto asperiores aut consequuntur
						corporis debitis ea eveniet in maiores, nam placeat quae, ratione rerum ullam?
					</p>
				</div>
			`;
		}

		const onCardClick = async (e) => {
			const card = e.currentTarget;
			// clone the card
			const cardClone = card.cloneNode(true);
			// get the location of the card in the view
			const {top, left, width, height} = card.getBoundingClientRect();
			// position the clone on top of the original
			cardClone.style.position = 'fixed';
			cardClone.style.top = top + 'px';
			cardClone.style.left = left + 'px';
			cardClone.style.width = width + 'px';
			cardClone.style.height = height + 'px';
			// hide the original card with opacity
			card.style.opacity = '0';
			// add card to the same container
			card.parentNode.appendChild(cardClone);
			// create a close button to handle the undo
			const closeButton = document.createElement('button');
			// position the close button top corner
			closeButton.style = `
				position: fixed;
				z-index: 10000;
				top: 35px;
				right: 35px;
				width: 35px;
				height: 35px;
				border-radius: 50%;
				background-color: #e25656;
			`;
			// attach click event to the close button
			closeButton.addEventListener('click', async () => {
				// remove the button on close
				closeButton.remove();
				// remove the display style so the original content is displayed right
				cardClone.style.removeProperty('display');
				cardClone.style.removeProperty('padding');
				// show original card content
				[...cardClone.children].forEach(child => child.style.removeProperty('display'));
				fadeContent(cardClone, '0');
				// shrink the card back to the original position and size
				await toggleExpansion(cardClone, {top: `${top}px`, left: `${left}px`, width: `${width}px`, height: `${height}px`}, 300)
				// show the original card again
				card.style.removeProperty('opacity');
				// remove the clone card
				cardClone.remove();
			});
			// fade the content away
			fadeContent(cardClone, '0')
				.then(() => {
					[...cardClone.children].forEach(child => child.style.display = 'none');
				});
			// expand the clone card
			await toggleExpansion(cardClone, {top: 0, left: 0, width: '100vw', height: '100vh'});
			const content = getCardContent(card.textContent, card.dataset.type)
			// set the display block so the content will follow the normal flow in case the original card is not display block
			cardClone.style.display = 'block';
			cardClone.style.padding = '0';
			// append the close button after the expansion is done
			cardClone.appendChild(closeButton);
			cardClone.insertAdjacentHTML('afterbegin', content);
		};

		cards.forEach(card => card.addEventListener('click', onCardClick));
	</script>


<body>
        <div class="wrapper">
			<header>
				<h1>Card Expand Animation</h1>
				<nav><span></span><span></span></nav>
				<p>How to animate card expand opening</p>
			</header>
			<div class="cards">
				<div class="card" data-type="html">
					<h2>HTML5</h2>
				</div>
				<div class="card" data-type="css">
					<h2>CSS3</h2>
				</div>
				<div class="card" data-type="mongo">
					<h2>Mongo</h2>
				</div>
				<div class="card" data-type="javascript">
					<h2>Javascript</h2>
				</div>
				<div class="card" data-type="nodejs">
					<h2>NodeJs</h2>
				</div>
				<div class="card" data-type="sql">
					<h2>SQL</h2>
				</div>
			</div>
	</div>
    </body>


	<style>
		body {
			background: linear-gradient(45deg, black, #ea3e3e);
			color: #ddd;
		}
		*, *::after, *::before {
		  box-sizing: border-box;
		}
		@keyframes slideDown {
			100%{
				transform: translateY(0);
				opacity: 1;
			}
		}
		.wrapper {
			padding: 0 25px;
			max-width: 425px;
			height: 100vh;
		}
		header {
			padding-top: 100px;
			margin-bottom: 50px;
		}
		header h1 {
			font-size: 42px;
			margin-bottom: 10px;
			color: #271111;
		}
		nav {
			position: absolute;
			top: 40px;
			right: 35px;
			width: 35px;
		}
		nav span {
			background-color: #222;
			margin: 4px 0;
			height: 3px;
			display: block;
		}

		.cards {
			display: flex;
			flex-wrap: wrap;
			justify-content: space-between;
			overflow: auto;
		}

		.cards .card {
			width: calc(50% - 13px);
			height: 150px;
			background-color: #222;
			color: #dc7b7b;
			margin-bottom: 20px;
			border-radius: 5px;
			cursor: pointer;
			display: flex;
			flex-direction: column;
			justify-content: flex-end;
			padding: 10px;
			border: 3px solid #e25656;
			box-shadow: 0 0 9px #4c0505;
		}

		.cards  .card h2 {
			margin: 0;
		}

		.card-content {
			padding: 100px 25px 25px;
			overflow: auto;
			height: 100%;
			color: #fff;
		}
		.card-content > * {
			transform: translateY(-35px);
			opacity: 0;
		}
		.card-content h2 {
			font-size: 32px;
			margin-bottom: 35px !important;
			animation: slideDown 0.5s ease-out forwards;
		}
		.card-content img {
			width: 100%;
			background-color: #fff;
			margin-bottom: 10px;
			animation: slideDown 0.5s ease-out 0.2s forwards;
		}
		.card-content p {
			color: #999;
			animation: slideDown 0.5s ease-out 0.4s forwards;
		}
	</style>


