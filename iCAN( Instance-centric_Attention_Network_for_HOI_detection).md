# iCAN( Instance-centric Attention Network for HOI detection)

https://arxiv.org/abs/1808.10437

Unlike modeling the visual scene context with convolutional kernel in iCAN, ***our work attempts to extract informative scene regions with human-centric cues***. Comparing the proposed iHOI with the best performing iCAN shows that iHOI can achieve overall better performance on most action-target categories, whereas showing slight worse performance on a small proportion of the categories such as hit-instr, talk-on-phone-instr, cut-instr. We observe that iHOI performs worse mostly on actions with small objects, possibly due to in-accurate object detection



iCAN은  HOI 계산 했고, 이 두번째 발표에서 HOI와 비교 할 때 사용 됐음

iCAN은 인간의 intention을 반영하지 않음

Our  iCAN  module  offers  several  advantages  over  existing  approaches.   First,  unlike hand-designed contextual features based on pose [6],  the entire image [31],  or secondary regions [13], our attention map are automatically learned and jointly trained with the rest of  the  networks  for  improving  the  performance.   Second,  when  compared  with attention modules designed for image-level classification, our instance-centric attention map provides greater flexibility as it allows attending to different regions in an image depending on different object instances.

