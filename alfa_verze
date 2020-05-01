from math import atan2, sqrt
import random


def cross_product(x1_1, y1_1, x1_2, y1_2, x2_1, y2_1, x2_2, y2_2):
	'''
	Funkce počítá vektorový součin dvou vektorů.

	:param x1_1: Hodnota x prvního bodu první proměnné
	:param y1_1: Hodnota y prvního bodu první proměnné
	:param x1_2: Hodnota x druhého bodu první proměnné
	:param y1_2: Hodnota y druhého bodu první proměnné
	:param x2_1: Hodnota x prvního bodu druhé proměnné
	:param y2_1: Hodnota y prvního bodu druhé proměnné
	:param x2_2: Hodnota x druhého bodu druhé proměnné
	:param y2_2: Hodnota y druhého bodu druhé proměnné
	:return: Vektorový součin
	'''
	first_vector_of_cross_product = [x1_1-x1_2, y1_1-y1_2] 	# Spočítá první vektor
	second_vector_of_cross_product = [x2_1-x2_2, y2_1-y2_2]	# Spočítá druhý vektor
	their_cross_product = first_vector_of_cross_product[0] * second_vector_of_cross_product[1]\
						- first_vector_of_cross_product[1] * second_vector_of_cross_product[0]
	return their_cross_product


def intersection_of_two_vectors(x1_1, y1_1, x1_2, y1_2, x2_1, y2_1, x2_2, y2_2):
	'''
	Funkce určuje, zda se protínají dva vektory, podle hodnot vektorových součinů.
	:param x1_1: Hodnota x prvního bodu první proměnné
	:param y1_1: Hodnota y prvního bodu první proměnné
	:param x1_2: Hodnota x druhého bodu první proměnné
	:param y1_2: Hodnota y druhého bodu první proměnné
	:param x2_1: Hodnota x prvního bodu druhé proměnné
	:param y2_1: Hodnota y prvního bodu druhé proměnné
	:param x2_2: Hodnota x druhého bodu druhé proměnné
	:param y2_2: Hodnota y druhého bodu druhé proměnné
	:return: True - vektory se protínají, False - vektory se neprotínají
	'''
	vector_1 = [x1_1-x1_2, y1_1-y1_2]
	vector_2 = [x2_1-x2_2, y2_1-y2_2]
	if vector_1[0]*vector_2[1]-vector_1[1]*vector_2[0] == 0:
		return False
	else:
		v1 = cross_product(x2_1, y2_1, x2_2, y2_2, x2_1, y2_1, x1_1, y1_1)
		v2 = cross_product(x2_1, y2_1, x2_2, y2_2, x2_1, y2_1, x1_2, y1_2)
		v3 = cross_product(x1_1, y1_1, x1_2, y1_2, x1_1, y1_1, x2_1, y2_1)
		v4 = cross_product(x1_1, y1_1, x1_2, y1_2, x1_1, y1_1, x2_2, y2_2)
		if v1*v2 < 0 and v3*v4 < 0:
			return True
		else:
			return False

def distance(p0, p1=None):
	'''
	Funkce spočítá délku mezi dvěma body.
	:param p0: První bod
	:param p1: Druhý bod
	:return: Délka mezi body
	'''
	if p1 == None:
		p1 = anchor
	y_span = p0[1] - p1[1]
	x_span = p0[0] - p1[0]
	return y_span**2 + x_span**2


def polar_angle(p0, p1=None):
	'''
	Funkce spočítá polární úhel s osou x
	:param p0: První bod
	:param p1: Druhý bod
	:return: Funkce vrátí tangens úhle mezi bodem a osou x
	'''
	if p1 == None:
		p1 = anchor
	y_span = p0[1] - p1[1]
	x_span = p0[0] - p1[0]
	return atan2(y_span, x_span)


def sort_polar_angle(s):
	'''
	Funkce třídí body podle jejich polárních úhlů
	:param s: Souřadnice všech bodů překážek
	:return: Seznam souřadnic
	'''
	if len(s) <= 1:
		return s
	smaller, equal, bigger = [], [], []
	random_anchor_angle = polar_angle(s[random.randint(0, len(s)-1)]) 		# choose random start of separating \
																			# vybirame nahodny start
	for element in s:
		element_angle = polar_angle(element)
		if element_angle < random_anchor_angle:
			smaller.append(element)
		elif element_angle == random_anchor_angle:
			equal.append(element)
		else:
			bigger.append(element)
	return sort_polar_angle(smaller) + sorted(equal, key=distance) + sort_polar_angle(bigger)


def det(p1,p2,p3):
	'''
	Funkce určuje, zda pohyb od bodu 1 k bodu 3 přes bod 2 šel po nebo proti směru hodinových ručíček.
	Pokud return bude < 0, tak pohyb jde po směru hodinových ručíček, pokud return > 0, tak pohyb jde proti.
	:param p1:
	:param p2:
	:param p3:
	:return:
	'''
	return   (p2[0]-p1[0])*(p3[1]-p1[1]) \
			-(p2[1]-p1[1])*(p3[0]-p1[0])


def graham_scan(points):
	'''
	Funkce určuje convexní obaly překážek podle toho, jestli pohyb těmito body jde po nebo proti chodu hodinocých
	ručíček.
	Potřebujeme jít jenom proti směru hodinových ručíček, abyjsme dpstali konvexní obaly, protože jsme setřidili
	body ve vzestupném pořádí polárního úhlu.
	:param points: Souřadnice překážek
	:return: Konvexní obaly
	'''
	global anchor
	min_id = None
	for i, (x, y) in enumerate(points):
		if min_id == None or y < points[min_id][1]:
			min_id = i
		if y == points[min_id][1] and x < points[min_id][0]:
			min_id =  i
	anchor = points[min_id]
	sorted_points = sort_polar_angle(points)
	del sorted_points[sorted_points.index(anchor)]
	konvex_obal = [anchor, sorted_points[0]]
	for s in sorted_points[1:]:
		while det(konvex_obal[-2], konvex_obal[-1], s) <= 0:
			del konvex_obal[-1]
			if len(konvex_obal) < 2:
				break
		konvex_obal.append(s)
	return konvex_obal


def convexni_obal(file_name):
	'''
	Funkce čte souřadnice ze souboru a počítá konvexní obaly každé překážky.
	:param file_name: Jméno souboru
	:return: Convexní obaly překážek
	'''
	coord = []
	length_of_each_string = []
	convexni_obaly = []

	with open(file_name) as pattern_file:
		for line in pattern_file:
			for i in range(len(line.split())):
				coord.append(list(line.split()[i].split()[0].split(',')))
			length_of_each_string.append(len(line.split()))

	for i in range(len(coord)):
		coord[i][0] = int(coord[i][0])
		coord[i][1] = int(coord[i][1])
	start_p = 0
	finish_p = length_of_each_string[0]
	for i in range(len(length_of_each_string)):
		convexni_obaly.append(graham_scan(coord[start_p:finish_p]))
		start_p += length_of_each_string[i]
		if (i+1) == len(length_of_each_string):
			break
		else:
			finish_p += length_of_each_string[i+1]
	return convexni_obaly


def cos_of_vektors(x1, y1, x2, y2):
	'''
	Funcke spočítá cosinus úhlu mezi dvěma vektory
	:param x1: Hodnota x prvního bodu
	:param y1: Hodnota y prvního bodu
	:param x2: Hodnota x druhého bodu
	:param y2: Hodnota y druhého bodu
	:return: cos úhlu
	'''
	cosin = (x1 * x2 + y1 * y2)/((sqrt(x1 * x1 + y1 * y1)) * (sqrt(x2 * x2 + y2 * y2)))
	return cosin


def available_bods(start, finish, prekazky):
	'''
	Funkce vybírá body, kterí leží mezi dvěma rovnoběžnými přímkami, které procházejí přes bod, kde se nachází robot,
	a bod konce pohybu.
	Pokud start a konec leží vně konvexního obalu nějaké překážky,nejkratší cesta může procházet jenom body této roviny.
	:param start: Start pohybu
	:param finish: Konec pohybu
	:param prekazky: Body konvexních obalů
	:return:
	'''
	available = []
	vektor_st_fin = [finish[0] - start[0], finish[1] - start[1]]
	vektor_fin_st = [start[0] - finish[0], start[1] - finish[1]]
	for i in range(len(prekazky)):
		available.append([])
		for k in range(len(prekazky[i])):
			st_point = [prekazky[i][k][0] - start[0], prekazky[i][k][1] - start[1]]
			f_point = [prekazky[i][k][0] - finish[0], prekazky[i][k][1] - finish[1]]
			if cos_of_vektors(vektor_st_fin[0], vektor_st_fin[1], st_point[0], st_point[1]) > 0 and \
					cos_of_vektors(vektor_fin_st[0], vektor_fin_st[1], f_point[0], f_point[1]) > 0:
				available[i].append(prekazky[i][k])
	return available


def vyber_koncovych_bodu(start, finish, st):
	'''
	Funkce spočítá koncové body z každé strany z hlediska robota.
	:param start: Poloha robota
	:param finish: Konec pohybu
	:param st: Konvexní obaly
	:return: Seznam souřadnic konocvých bodů pro každou překážku.
	'''
	prekazky = available_bods([1, 20], [22, 2], st)
	konc_body = []
	for i in range(len(prekazky)):
		konc_body.append([])
		anchor = prekazky[i][random.randint(0, len(prekazky[i])-1)]
		v_anchor = [anchor[0] - start[0], anchor[1]- start[1]]
		min_cos = 1
		for k in range(len(prekazky[i])):
			v_k = [prekazky[i][k][0] - start[0], prekazky[i][k][1] - start[1]]
			if cos_of_vektors(v_anchor[0], v_anchor[1], v_k[0], v_k[1]) < min_cos:
				min_cos = cos_of_vektors(v_anchor[0], v_anchor[1], v_k[0], v_k[1])
				bod_1 = prekazky[i][k]
				v_bod_1 = [bod_1[0] - start[0], bod_1[1] - start[1]]
		min_cos = 1
		for k in range(len(prekazky[i])):
			v_k = [prekazky[i][k][0] - start[0], prekazky[i][k][1] - start[1]]
			if cos_of_vektors(v_bod_1[0], v_bod_1[1], v_k[0], v_k[1]) < min_cos:
				min_cos = cos_of_vektors(v_bod_1[0], v_bod_1[1], v_k[0], v_k[1])
				bod_2 = prekazky[i][k]
		konc_body[i].extend([bod_1, bod_2])
	return konc_body


def delka_mezi_body(x1, y1, x2, y2):
	'''
	Funcke spočítá délku mezi dvěma body.
	:param x1: Hodnota x prvního bodu
	:param y1: Hodnota y prvního bodu
	:param x2: Hodnota x druhého bodu
	:param y2: Hodnota y druhého bodu
	:return: Délka
	'''
	delka = sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1))
	return delka


def our_sides_of_figures(st, start, finish):
	'''
	Funkce nejprve rozdělí konvexní obaly všech překážek na dvě strany, které leží mezi koncovými body překážky.
	A pak vybírá, která strana je bližší.
	:param st: Všechny souřadnice překážek.
	:param start: Poloha robota
	:param finish: Konec pohybu
	:return: Bližší strany každé překážky.
	'''
	dostupne_body = available_bods(start, finish, st)
	koncove_body = vyber_koncovych_bodu(start, finish, st)
	obe_strany = []
	viditelne_body = []
	for i in range(len(koncove_body)):
		obe_strany.append([])
		side_1 = []
		side_2 = []
		if dostupne_body[i].index(koncove_body[i][0]) > dostupne_body[i].index(koncove_body[i][1]):
			side_1 = dostupne_body[i][dostupne_body[i].index(koncove_body[i][0]) : -1] + \
				[dostupne_body[i][-1]] + dostupne_body[i][0 : dostupne_body[i].index(koncove_body[i][1])+1]
			side_2 = dostupne_body[i][dostupne_body[i].index(koncove_body[i][1]) \
									  : dostupne_body[i].index(koncove_body[i][0])+1]
		elif dostupne_body[i].index(koncove_body[i][0]) < dostupne_body[i].index(koncove_body[i][1]):
			side_1 = dostupne_body[i][dostupne_body[i].index(koncove_body[i][0]) \
									  : dostupne_body[i].index(koncove_body[i][1])+1]
			side_2 = dostupne_body[i][dostupne_body[i].index(koncove_body[i][1]) : -1] + \
				[dostupne_body[i][-1]] + dostupne_body[i][0 : dostupne_body[i].index(koncove_body[i][0])+1]
		obe_strany[i].extend([side_1, side_2])
	print(obe_strany)

	for i in range(len(obe_strany)):
		viditelne_body.append([])
		if len(obe_strany[i][0]) > 2 and len(obe_strany[i][1]) > 2:
			if intersection_of_two_vectors(obe_strany[i][0][1][0], obe_strany[i][0][1][1], start[0], start[1], \
										   koncove_body[i][0][0], koncove_body[i][0][1], \
										   koncove_body[i][1][0], koncove_body[i][1][1]) \
				and \
				not intersection_of_two_vectors(obe_strany[i][1][1][0], obe_strany[i][1][1][1], start[0], start[1], \
											koncove_body[i][0][0], koncove_body[i][0][1], \
											koncove_body[i][1][0], koncove_body[i][1][1]):
				viditelne_body[i].extend(obe_strany[i][1])
			elif intersection_of_two_vectors(obe_strany[i][1][1][0], obe_strany[i][1][1][1], start[0], start[1], \
											koncove_body[i][0][0], koncove_body[i][0][1], \
											koncove_body[i][1][0], koncove_body[i][1][1]) \
				and \
				not intersection_of_two_vectors(obe_strany[i][0][1][0], obe_strany[i][0][1][1], start[0], start[1], \
										   koncove_body[i][0][0], koncove_body[i][0][1], \
										   koncove_body[i][1][0], koncove_body[i][1][1]) :
				viditelne_body[i].extend(obe_strany[i][0])
		elif len(obe_strany[i][0]) > 2 and len(obe_strany[i][1]) <= 2:
			if intersection_of_two_vectors(obe_strany[i][0][1][0], obe_strany[i][0][1][1], start[0], start[1], \
										   koncove_body[i][0][0], koncove_body[i][0][1], \
										   koncove_body[i][1][0], koncove_body[i][1][1]):
				viditelne_body[i].extend(obe_strany[i][1])
			else:
				viditelne_body[i].extend(obe_strany[i][0])
		elif len(obe_strany[i][1]) > 2 and len(obe_strany[i][0]) <= 2:
			if intersection_of_two_vectors(obe_strany[i][1][1][0], obe_strany[i][1][1][1], start[0], start[1], \
										   koncove_body[i][0][0], koncove_body[i][0][1], \
										   koncove_body[i][1][0], koncove_body[i][1][1]):
				viditelne_body[i].extend(obe_strany[i][0])
			else:
				viditelne_body[i].extend(obe_strany[i][1])
	return viditelne_body


def global_kon_body(start, body):
	'''
	Funkce vybírá koncové body ze všech bodů všech překážek z hlediska robota.
	:param start: Poloha robota
	:param body: Vsechny souřadnice
	:return:
	'''
	angel_points = []
	viditelne_body = []
	for i in range(len(body)):
		for k in range(len(body[i])):
			viditelne_body.append(body[i][k])
	konc_body = []
	anchor = viditelne_body[random.randint(0, len(viditelne_body) - 1)]
	v_anchor = [anchor[0] - start[0], anchor[1] - start[1]]
	min_cos = 1
	for i in range(len(viditelne_body)):
		v_k = [viditelne_body[i][0] - start[0], viditelne_body[i][1] - start[1]]
		if cos_of_vektors(v_anchor[0], v_anchor[1], v_k[0], v_k[1]) < min_cos:
			min_cos = cos_of_vektors(v_anchor[0], v_anchor[1], v_k[0], v_k[1])
			bod_1 = viditelne_body[i]
	v_bod_1 = [bod_1[0] - start[0], bod_1[1] - start[1]]
	min_cos = 1
	for k in range(len(viditelne_body)):
		v_k = [viditelne_body[k][0] - start[0], viditelne_body[k][1] - start[1]]
		if cos_of_vektors(v_bod_1[0], v_bod_1[1], v_k[0], v_k[1]) < min_cos:
			min_cos = cos_of_vektors(v_bod_1[0], v_bod_1[1], v_k[0], v_k[1])
			bod_2 = viditelne_body[k]
	konc_body.extend([bod_1, bod_2])
	for i in range(len(viditelne_body)):
		if cross_product(start[0], start[1], konc_body[0][0], konc_body[0][1], \
						 start[0], start[1], viditelne_body[i][0], viditelne_body[i][1]) == 0:
			if delka_mezi_body(start[0], start[1], konc_body[0][0], konc_body[0][1]) < \
				delka_mezi_body(start[0], start[1], viditelne_body[i][0], viditelne_body[i][1]):
				continue
			elif delka_mezi_body(start[0], start[1], konc_body[0][0], konc_body[0][1]) > \
				delka_mezi_body(start[0], start[1], viditelne_body[i][0], viditelne_body[i][1]):
				konc_body[0] = viditelne_body[i]
		if cross_product(start[0], start[1], konc_body[1][0], konc_body[1][1], \
						 start[0], start[1], viditelne_body[i][0], viditelne_body[i][1]) == 0:
			if delka_mezi_body(start[0], start[1], konc_body[1][0], konc_body[1][1]) < \
				delka_mezi_body(start[0], start[1], viditelne_body[i][0], viditelne_body[i][1]):
				continue
			elif delka_mezi_body(start[0], start[1], konc_body[1][0], konc_body[1][1]) > \
				delka_mezi_body(start[0], start[1], viditelne_body[i][0], viditelne_body[i][1]):
				konc_body[1] = viditelne_body[i]
	angel_points.extend([bod_1, bod_2])
	v_a_1 = [angel_points[0][0] - start[0], angel_points[0][1] - start[1]]
	for i in range(len(viditelne_body)):
		v_a_2 = [angel_points[1][0] - start[0], angel_points[1][1] - start[1]]
		v_k = [viditelne_body[i][0] - start[0], viditelne_body[i][1] - start[1]]
		if  cos_of_vektors(v_a_1[0], v_a_1[1],v_a_2[0], v_a_2[1]) < \
			cos_of_vektors(v_a_1[0], v_a_1[1], v_k[0], v_k[1]):
			angel_points.insert(1, viditelne_body[i])
	return konc_body, angel_points


def sin_of_vectors(x1, y1, x2, y2):
	'''
	Funkce spočítá sin úhlu mezi dvěma vektory.
	:param x1: Hodnota x prvního bodu
	:param y1: Hodnota y prvního bodu
	:param x2: Hodnota x druhého bodu
	:param y2: Hodnota y druhého bodu
	:return: Sin úhlu
	'''
	return sqrt(1 - cos_of_vektors(x1, y1, x2, y2))


def main():
	st = convexni_obal('coordinates')
	dostupne_body = available_bods([1, 20], [22, 2], st)
	konc_body = vyber_koncovych_bodu([1, 20], [22, 2], dostupne_body)
	strany = our_sides_of_figures(st, [1, 20], [20, 2])
	print(dostupne_body)
	print(konc_body)
	print(strany)


if __name__== "__main__":
  main()

# Program jsme nestihli dokončit, ale byli jsme blízko ke konečné verzi. Zatím náš robot umí spočítat konvexní
# obaly každé překážky, určit koncové body každé překážky, které robot "vidí", určit, které body jsou v rovině mezi
# ním a koncem jeho pohybu. Plánovali jsme, že pak bude robot určovat, do kterých bodů se může pohybovat, počítat
# délku každé cesty a z toho vybírat tu nejkratší.
