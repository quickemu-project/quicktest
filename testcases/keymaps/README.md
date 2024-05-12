# Qemu Keymaps

These are required because Qemu has a limited set of 'keys' it supports
when sending keypresses via the monitor.

We use these keymaps because some 'keys' like `#` are available only via
keyboard combinations such as `shift-3` in an en_US layout and `£` on
the same key combination in the en_GB keymap.

As of QEMU 0.12.5 the supported keys are:


```
shift	shift_r	alt	alt_r	altgr	altgr_r
ctrl	ctrl_r	menu	esc	1	2
3	4	5	6	7	8
9	0	minus	equal	backspace	tab
q	w	e	r	t	y
u	i	o	p	ret	a
s	d	f	g	h	j
k	l	z	x	c	v
b	n	m	comma	dot	slash
asterisk	spc	caps_lock	f1	f2	f3
f4	f5	f6	f7	f8	f9
f10	num_lock	scroll_lock	kp_divide	kp_multiply	kp_subtract
kp_add	kp_enter	kp_decimal	sysrq	kp_0	kp_1
kp_2	kp_3	kp_4	kp_5	kp_6	kp_7
kp_8	kp_9	<	f11	f12	print
home	pgup	pgdn	end	left	up
down	right	insert	delete
```
