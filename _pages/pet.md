---
layout: page
title: Director of Wellness
permalink: /pet/
nav: false
pet_images: [{ src: /assets/img/laifu.jpeg, date: "2026-02-01" }, { src: /assets/img/laifu2.jpeg, date: "2026-02-10" }, { src: /assets/img/laifu3.jpeg, date: "2026-02-18" }]
---

<div class="row g-4 align-items-start">
	<div class="col-lg-7">
		<h2>Laifu (来福)</h2>

		<p><strong>Director of Health, Wellness, and Everyday Brilliance</strong></p>

		<p>"While his official duties include mandating outdoor walks and interrupting overly long coding sessions, Laifu’s true role is making life brilliant. This little Schnauzer brings endless color, happiness, and perspective to my days."</p>
	</div>

	<div class="col-lg-5">
		<div class="rounded overflow-hidden border position-relative">
			<img id="laifu-image" src="{% if page.pet_images[0].src %}{{ page.pet_images[0].src }}{% else %}{{ page.pet_images[0] }}{% endif %}" class="d-block w-100" alt="Laifu photo 1">
			<span id="laifu-date" class="position-absolute top-0 end-0 m-2 badge" style="background: rgba(0, 0, 0, 0.75); color: #fff;">{% if page.pet_images[0].date %}{{ page.pet_images[0].date | date: "%Y-%m-%d" }}{% endif %}</span>
		</div>
		<div class="d-flex justify-content-between align-items-center mt-2">
			<button id="laifu-prev" type="button" class="btn btn-sm btn-outline-secondary">Previous</button>
			<span id="laifu-count" class="small text-muted"></span>
			<button id="laifu-next" type="button" class="btn btn-sm btn-outline-secondary">Next</button>
		</div>
	</div>
</div>

<script>
	document.addEventListener("DOMContentLoaded", function () {
		const rawImages = {{ page.pet_images | jsonify }};
		const images = (rawImages || [])
			.map(function (item) {
				if (typeof item === "string") {
					return { src: item, date: "" };
				}
				const normalizedDate = item && item.date ? String(item.date).slice(0, 10) : "";
				return { src: item && item.src ? item.src : "", date: normalizedDate };
			})
			.filter(function (item) {
				return item.src;
			});
		const imageEl = document.getElementById("laifu-image");
		const dateEl = document.getElementById("laifu-date");
		const prevBtn = document.getElementById("laifu-prev");
		const nextBtn = document.getElementById("laifu-next");
		const countEl = document.getElementById("laifu-count");
		if (!imageEl || !dateEl || !prevBtn || !nextBtn || !countEl || images.length === 0) return;

		let current = 0;
		const update = function () {
			imageEl.src = images[current].src;
			imageEl.alt = "Laifu photo " + (current + 1);
			if (images[current].date) {
				dateEl.textContent = images[current].date;
				dateEl.style.display = "inline-block";
			} else {
				dateEl.style.display = "none";
			}
			countEl.textContent = (current + 1) + " / " + images.length;
			const disabled = images.length === 1;
			prevBtn.disabled = disabled;
			nextBtn.disabled = disabled;
		};

		prevBtn.addEventListener("click", function () {
			current = (current - 1 + images.length) % images.length;
			update();
		});

		nextBtn.addEventListener("click", function () {
			current = (current + 1) % images.length;
			update();
		});

		update();
	});
</script>
