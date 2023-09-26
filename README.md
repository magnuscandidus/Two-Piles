# Two-Piles
#include <algorithm>
#include <functional>
#include <iostream>
#include <utility>
#include <vector>

static void testcase() {
	unsigned N;
	std::cin >> N;
	std::vector<std::pair<int, unsigned>> vip;
	vip.reserve(2 * N);
	int max_at_least = 0;
	for(unsigned i = 0; i != N; ++i) {
		int Ai, Bi;
		std::cin >> Ai >> Bi;
		vip.emplace_back(Ai, i);
		vip.emplace_back(Bi, i);
		max_at_least = std::max(max_at_least, std::min(Ai, Bi));
	}
	auto first_too_small = std::partition(vip.begin(), vip.end(), [&](const auto& vi) {
			return vi.first >= max_at_least;
		});
	auto potential_maxes = first_too_small - vip.begin();
	std::partial_sort(vip.begin(), vip.begin() + potential_maxes + 2, vip.end(), std::greater{});
	int best_diff = vip[0].first;
	for(unsigned i = 0; i != potential_maxes; ++i)
		best_diff = std::min(best_diff, vip[i].first - vip[i + 1 + (vip[i].second == vip[i + 1].second)].first);
	std::cout << best_diff << '\n';
}

int main() {
	std::ios::sync_with_stdio(false);
	std::cin.tie(nullptr);
	int T;
	std::cin >> T;
	while(T--)
		testcase();
}
